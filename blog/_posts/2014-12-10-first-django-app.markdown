---
layout: post
title: Writing your first Django app
---
### 1. Throughout this tutorial, we’ll walk you through the creation of a basic poll application.
---
#### Check Django is installed and which version ([Setup Django](/blog/2014/12/06/setup-django/)):
{% highlight bash %}
$ python -c "import django; print(django.get_version())"
{% endhighlight %}

#### Creating a project:
{% highlight bash %}
$ django-admin startproject mysite
{% endhighlight %}

#### Database setup:
Edit `mysite/settings.py`:
{% highlight python %}
DATABASES = {
  'default': {
    'ENGINE': 'django.db.backends.mysql',
    'NAME': 'mysite',
    'USER': 'root',
    'PASSWORD': '',
    'HOST': 'localhost',
    'PORT': '3306',
  }
}
{% endhighlight %}
{% highlight bash %}
$ python manage.py migrate
{% endhighlight %}

#### The development server:
{% highlight bash %}
$ python manage.py runserver
{% endhighlight %}
Changing the port:
{% highlight bash %}
$ python manage.py runserver 8080
$ python manage.py runserver 0.0.0.0:8000
{% endhighlight %}

#### Creating models:
{% highlight bash %}
$ python manage.py startapp polls
{% endhighlight %}
Edit the `polls/models.py`:
{% highlight py %}
from django.db import models

class Question(models.Model):
	question_text = models.CharField(max_length=200)
	pub_date = models.DateTimeField('date published')

class Choice(models.Model):
	question = models.ForeignKey(Question)
	choice_text = models.CharField(max_length=200)
	votes = models.IntegerField(default=0)
{% endhighlight %}

#### Activating models:
Edit the `mysite/settings.py` file again:
{% highlight py %}
INSTALLED_APPS = (
	'django.contrib.admin',
	'django.contrib.auth',
	'django.contrib.contenttypes',
	'django.contrib.sessions',
	'django.contrib.messages',
	'django.contrib.staticfiles',
	'polls',
)
{% endhighlight %}
{% highlight bash %}
$ python manage.py makemigrations polls
$ python manage.py sqlmigrate polls 0001
$ python manage.py migrate
{% endhighlight %}

#### Playing with the API:
{% highlight bash %}
$ python manage.py shell

>>> from polls.models import Question, Choice
>>> Question.objects.all()
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save()
>>> q.id
>>> q.question_text
>>> q.pub_date
>>> q.question_text = "What's up?"
>>> q.save()
>>> Question.objects.all()
{% endhighlight %}

Edit `polls/models.py`, add functions:
{% highlight py %}
import datetime

from django.db import models
from django.utils import timezone

class Question(models.Model):
	# ...
	def __str__(self):              # __unicode__ on Python 2
		return self.question_text

	def was_published_recently(self):
		return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

class Choice(models.Model):
	# ...
	def __str__(self):              # __unicode__ on Python 2
		return self.choice_text
{% endhighlight %}
Run `python manage.py shell` again:
{% highlight bash %}
>>> from polls.models import Question, Choice
>>> Question.objects.all()
>>> Question.objects.filter(id=1)
>>> Question.objects.filter(question_text__startswith='What')
>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
>>> Question.objects.get(id=2)
>>> Question.objects.get(pk=1)
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
>>> q = Question.objects.get(pk=1)
>>> q.choice_set.all()
>>> q.choice_set.create(choice_text='Not much', votes=0)
>>> q.choice_set.create(choice_text='The sky', votes=0)
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)
>>> c.question
>>> q.choice_set.all()
>>> q.choice_set.count()
>>> Choice.objects.filter(question__pub_date__year=current_year)
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
{% endhighlight %}

### 2. We’re continuing the Web-poll application and will focus on Django’s automatically-generated admin site.
---
#### Creating an admin user:
{% highlight bash %}
$ python manage.py createsuperuser
Username: admin
Email address: admin@example.com
Password: **********
Password (again): *********
Superuser created successfully.
{% endhighlight %}

#### Start the development server:
{% highlight bash %}
$ python manage.py runserver
{% endhighlight %}
Open [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)

#### Make the poll app modifiable in the admin:
`polls/admin.py`:
{% highlight python %}
from django.contrib import admin
from polls.models import Question

admin.site.register(Question)
{% endhighlight %}

#### Customize the admin form:
`polls/admin.py`:
{% highlight python %}
from django.contrib import admin
from polls.models import Question

// admin.site.register(Question)
class QuestionAdmin(admin.ModelAdmin):
	fields = ['pub_date', 'question_text']
// or
class QuestionAdmin(admin.ModelAdmin):
	fieldsets = [
		(None,               {'fields': ['question_text']}),
		('Date information', {'fields': ['pub_date'], 'classes': ['collapse']}),
	]

admin.site.register(Question, QuestionAdmin)
{% endhighlight %}

#### Adding related objects:
`polls/admin.py`:
{% highlight python %}
from django.contrib import admin
from polls.models import Choice, Question
# ...
admin.site.register(Choice)
// or
from django.contrib import admin
from polls.models import Choice, Question

class ChoiceInline(admin.StackedInline):
	model = Choice
	extra = 3

class QuestionAdmin(admin.ModelAdmin):
	fieldsets = [
		(None,               {'fields': ['question_text']}),
		('Date information', {'fields': ['pub_date'], 'classes': ['collapse']}),
	]
	inlines = [ChoiceInline]

admin.site.register(Question, QuestionAdmin)
{% endhighlight %}

#### Customize the admin change list:
`polls/admin.py`:
{% highlight python %}
class QuestionAdmin(admin.ModelAdmin):
	# ...
	list_display = ('question_text', 'pub_date', 'was_published_recently')
	list_filter = ['pub_date']
	search_fields = ['question_text']
{% endhighlight %}

#### Customize the admin look and feel:
`mysite/settings.py`:
{% highlight python %}
TEMPLATE_DIRS = [os.path.join(BASE_DIR, 'templates')]
{% endhighlight %}
{% highlight bash %}
$ python -c "
import sys
sys.path = sys.path[1:]
import django
print(django.__path__)"
$ cp /Library/Python/2.7/site-packages/django/contrib/admin/templates/admin/base_site.html ~/mysite/templates/admin
{% endhighlight %}
Replace `{ { site_header|default:_('Django administration') } }` in `base_site.html`:
{% highlight html %}
<h1 id="site-name"><a href="{ % url 'admin:index' % }">Polls Administration</a></h1>
{% endhighlight %}

### 3. We’re continuing the Web-poll application and will focus on creating the public interface – "views."
#### Write your first view:
Open the file `polls/views.py`:
{% highlight python %}
from django.http import HttpResponse

def index(request):
	return HttpResponse("Hello, world. You're at the polls index.")
{% endhighlight %}
Create a file called `urls.py` in polls app, `polls/urls.py`:
{% highlight python %}
from django.conf.urls import url
from polls import views

urlpatterns = [
	url(r'^$', views.index, name='index'),
]
{% endhighlight %}

#### Writing more views:
`polls/views.py`:
{% highlight python %}
def detail(request, question_id):
	return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
	response = "You're looking at the results of question %s."
	return HttpResponse(response % question_id)

def vote(request, question_id):
	return HttpResponse("You're voting on question %s." % question_id)
{% endhighlight %}
`polls.urls`:
{% highlight python %}
from django.conf.urls import url
from polls import views

urlpatterns = [
	# ex: /polls/
	url(r'^$', views.index, name='index'),
	# ex: /polls/5/
	url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
	# ex: /polls/5/results/
	url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
	# ex: /polls/5/vote/
	url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
{% endhighlight %}

#### Write views that actually do something:
`polls/views.py`:
{% highlight python %}
from django.http import HttpResponse
from polls.models import Question

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    output = ', '.join([p.question_text for p in latest_question_list])
    return HttpResponse(output)
{% endhighlight %}
Create a file called `templates/polls/index.html` in your `polls` directory, `polls/templates/polls/index.html`:
{% highlight html %}
{ % if latest_question_list % }
	<ul>
	{ % for question in latest_question_list % }
		<li><a href="/polls/{ { question.id } }/">{ { question.question_text } }</a></li>
	{ % endfor % }
	</ul>
{ % else % }
	<p>No polls are available.</p>
{ % endif % }
{% endhighlight %}
`polls/views.py`:
{% highlight python %}
from django.http import HttpResponse
from django.template import RequestContext, loader
from polls.models import Question

def index(request):
	latest_question_list = Question.objects.order_by('-pub_date')[:5]

	template = loader.get_template('polls/index.html')
	context = RequestContext(request, {
	    'latest_question_list': latest_question_list,
	})
	return HttpResponse(template.render(context))
	// or
	context = {'latest_question_list': latest_question_list}
	return render(request, 'polls/index.html', context)
{% endhighlight %}

#### Raising a 404 error:
`polls/views.py`:
{% highlight python %}
from django.http import Http404
from django.shortcuts import render
from polls.models import Question
# ...
def detail(request, question_id):
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404
    return render(request, 'polls/detail.html', {'question': question})
{% endhighlight %}
`polls/templates/polls/detail.html`:
{% highlight html %}
{ { question } }
{% endhighlight %}
A shortcut: `get_object_or_404()`, `polls/views.py`:
{% highlight python %}
from django.shortcuts import get_object_or_404, render
from polls.models import Question
# ...
def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/detail.html', {'question': question})
{% endhighlight %}

#### Use the template system:
`polls/templates/polls/detail.html`:
{% highlight html %}
<h1>{ { question.question_text } }</h1>
<ul>
{ % for choice in question.choice_set.all % }
	<li>{ { choice.choice_text } }</li>
{ % endfor % }
</ul>
{% endhighlight %}

#### Removing hardcoded URLs in templates:
`polls/templates/polls/index.html`:
{% highlight html %}
<li><a href="/polls/{ { question.id } }/">{ { question.question_text } }</a></li>
<!-- to -->
<li><a href="{ % url 'detail' question.id % }">{ { question.question_text } }</a></li>
{% endhighlight %}

#### Namespacing URL names:
`mysite/urls.py`:
{% highlight python %}
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^polls/', include('polls.urls', namespace="polls")),
    url(r'^admin/', include(admin.site.urls)),
]
{% endhighlight %}
`polls/templates/polls/index.html`:
{% highlight html %}
<li><a href="{ % url 'detail' question.id % }">{ { question.question_text } }</a></li>
<!-- to -->
<li><a href="{ % url 'polls:detail' question.id % }">{ { question.question_text } }</a></li>
{% endhighlight %}

### 4. We’re continuing the Web-poll application and will focus on simple form processing and cutting down our code.
#### Write a simple form:
`polls/templates/polls/detail.html`:
{% highlight html %}
<h1>{ { question.question_text } }</h1>
{ % if error_message % }<p><strong>{{ error_message }}</strong></p>{ % endif % }

<form action="{ % url 'polls:vote' question.id % }" method="post">
{ % csrf_token % }
{ % for choice in question.choice_set.all % }
    <input type="radio" name="choice" id="choice{ { forloop.counter } }" value="{ { choice.id } }" />
    <label for="choice{ { forloop.counter } }">{ { choice.choice_text } }</label><br />
{ % endfor % }
<input type="submit" value="Vote" />
</form>
{% endhighlight %}
`polls/views.py`:
{% highlight python %}
from django.shortcuts import get_object_or_404, render
from django.http import HttpResponseRedirect, HttpResponse
from django.core.urlresolvers import reverse

from polls.models import Choice, Question
# ...
def vote(request, question_id):
	p = get_object_or_404(Question, pk=question_id)
	try:
		selected_choice = p.choice_set.get(pk=request.POST['choice'])
	except (KeyError, Choice.DoesNotExist):
		return render(request, 'polls/detail.html', {
			'question': p,
			'error_message': "You didn't select a choice.",
		})
	else:
		selected_choice.votes += 1
		selected_choice.save()
		return HttpResponseRedirect(reverse('polls:results', args=(p.id,)))
{% endhighlight %}
`polls/views.py`:
{% highlight python %}
def results(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/results.html', {'question': question})
{% endhighlight %}
Create a `polls/results.html` template:
{% highlight html %}
<h1>{ { question.question_text } }</h1>

<ul>
{ % for choice in question.choice_set.all % }
    <li>{ { choice.choice_text } } -- { { choice.votes } } vote{ { choice.votes|pluralize } }</li>
{ % endfor % }
</ul>

<a href="{ % url 'polls:detail' question.id % }">Vote again?</a>
{% endhighlight %}

#### Amend URLconf:
First, open the `polls/urls.py` URLconf and change it like so:
{% highlight python %}
from django.conf.urls import url
from polls import views

urlpatterns = [
    url(r'^$', views.IndexView.as_view(), name='index'),
    url(r'^(?P<pk>[0-9]+)/$', views.DetailView.as_view(), name='detail'),
    url(r'^(?P<pk>[0-9]+)/results/$', views.ResultsView.as_view(), name='results'),
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
{% endhighlight %}

#### Amend views:
Next, we’re going to remove our old index, detail, and results views and use Django’s generic views instead. To do so, open the `polls/views.py` file and change it like so:
{% highlight python %}
from django.shortcuts import get_object_or_404, render
from django.http import HttpResponseRedirect
from django.core.urlresolvers import reverse
from django.views import generic
from polls.models import Choice, Question

class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by('-pub_date')[:5]

class DetailView(generic.DetailView):
    model = Question
    template_name = 'polls/detail.html'

class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'

def vote(request, question_id):
    ... # same as above
{% endhighlight %}

### 5. We’ve built a Web-poll application, and we’ll now create some automated tests for it.
#### Create a test to expose the bug:
Put the following in the `tests.py` file in the polls application:
{% highlight python %}
import datetime

from django.utils import timezone
from django.test import TestCase

from polls.models import Question

class QuestionMethodTests(TestCase):

    def test_was_published_recently_with_future_question(self):
        """
        was_published_recently() should return False for questions whose
        pub_date is in the future
        """
        time = timezone.now() + datetime.timedelta(days=30)
        future_question = Question(pub_date=time)
        self.assertEqual(future_question.was_published_recently(), False)
{% endhighlight %}

#### Running tests:
{% highlight bash %}
$ python manage.py test polls
{% endhighlight %}

#### Fixing the bug:
`polls/models.py`:
{% highlight python %}
def was_published_recently(self):
    now = timezone.now()
    return now - datetime.timedelta(days=1) <= self.pub_date <= now
{% endhighlight %}

#### More comprehensive tests:
`polls/tests.py`:
{% highlight python %}
def test_was_published_recently_with_old_question(self):
    """
    was_published_recently() should return False for questions whose
    pub_date is older than 1 day
    """
    time = timezone.now() - datetime.timedelta(days=30)
    old_question = Question(pub_date=time)
    self.assertEqual(old_question.was_published_recently(), False)

def test_was_published_recently_with_recent_question(self):
    """
    was_published_recently() should return True for questions whose
    pub_date is within the last day
    """
    time = timezone.now() - datetime.timedelta(hours=1)
    recent_question = Question(pub_date=time)
    self.assertEqual(recent_question.was_published_recently(), True)
{% endhighlight %}

{% highlight bash %}
$ python manage.py shell

>>> from django.test.utils import setup_test_environment
>>> setup_test_environment()
>>> from django.test import Client
>>> client = Client()
>>> response = client.get('/')
>>> response.status_code
>>> from django.core.urlresolvers import reverse
>>> response = client.get(reverse('polls:index'))
>>> response.status_code
>>> response.content
>>> from polls.models import Question
>>> from django.utils import timezone
>>> q = Question(question_text="Who is your favorite Beatle?", pub_date=timezone.now())
>>> q.save()
>>> response = client.get('/polls/')
>>> response.content
>>> response.context['latest_question_list']
{% endhighlight %}

#### Improving our view:
`polls/views.py`:
{% highlight python %}
from django.utils import timezone

def get_queryset(self):
    """
    Return the last five published questions (not including those set to be
    published in the future).
    """
    return Question.objects.filter(
        pub_date__lte=timezone.now()
    ).order_by('-pub_date')[:5]
{% endhighlight %}

#### Testing our new view:
Add the following to `polls/tests.py`:
{% highlight python %}
from django.core.urlresolvers import reverse

def create_question(question_text, days):
    """
    Creates a question with the given `question_text` published the given
    number of `days` offset to now (negative for questions published
    in the past, positive for questions that have yet to be published).
    """
    time = timezone.now() + datetime.timedelta(days=days)
    return Question.objects.create(question_text=question_text,
                                   pub_date=time)


class QuestionViewTests(TestCase):
    def test_index_view_with_no_questions(self):
        """
        If no questions exist, an appropriate message should be displayed.
        """
        response = self.client.get(reverse('polls:index'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, "No polls are available.")
        self.assertQuerysetEqual(response.context['latest_question_list'], [])

    def test_index_view_with_a_past_question(self):
        """
        Questions with a pub_date in the past should be displayed on the
        index page
        """
        create_question(question_text="Past question.", days=-30)
        response = self.client.get(reverse('polls:index'))
        self.assertQuerysetEqual(
            response.context['latest_question_list'],
            ['<Question: Past question.>']
        )

    def test_index_view_with_a_future_question(self):
        """
        Questions with a pub_date in the future should not be displayed on
        the index page.
        """
        create_question(question_text="Future question.", days=30)
        response = self.client.get(reverse('polls:index'))
        self.assertContains(response, "No polls are available.",
                            status_code=200)
        self.assertQuerysetEqual(response.context['latest_question_list'], [])

    def test_index_view_with_future_question_and_past_question(self):
        """
        Even if both past and future questions exist, only past questions
        should be displayed.
        """
        create_question(question_text="Past question.", days=-30)
        create_question(question_text="Future question.", days=30)
        response = self.client.get(reverse('polls:index'))
        self.assertQuerysetEqual(
            response.context['latest_question_list'],
            ['<Question: Past question.>']
        )

    def test_index_view_with_two_past_questions(self):
        """
        The questions index page may display multiple questions.
        """
        create_question(question_text="Past question 1.", days=-30)
        create_question(question_text="Past question 2.", days=-5)
        response = self.client.get(reverse('polls:index'))
        self.assertQuerysetEqual(
            response.context['latest_question_list'],
            ['<Question: Past question 2.>', '<Question: Past question 1.>']
        )
{% endhighlight %}

#### Testing the DetailView:
`polls/views.py`:
{% highlight python %}
class DetailView(generic.DetailView):
    ...
    def get_queryset(self):
        """
        Excludes any questions that aren't published yet.
        """
        return Question.objects.filter(pub_date__lte=timezone.now())
{% endhighlight %}
`polls/tests.py`:
{% highlight python %}
class QuestionIndexDetailTests(TestCase):
    def test_detail_view_with_a_future_question(self):
        """
        The detail view of a question with a pub_date in the future should
        return a 404 not found.
        """
        future_question = create_question(question_text='Future question.', days=5)
        response = self.client.get(reverse('polls:detail', args=(future_question.id,)))
        self.assertEqual(response.status_code, 404)

    def test_detail_view_with_a_past_question(self):
        """
        The detail view of a question with a pub_date in the past should
        display the question's text.
        """
        past_question = create_question(question_text='Past Question.', days=-5)
        response = self.client.get(reverse('polls:detail', args=(past_question.id,)))
        self.assertContains(response, past_question.question_text, status_code=200)
{% endhighlight %}

{% highlight python %}
{% endhighlight %}

{% highlight python %}
{% endhighlight %}

#### Resource:
* [Writing your first Django app, part 1](https://docs.djangoproject.com/en/dev/intro/tutorial01/)
* [Writing your first Django app, part 2](https://docs.djangoproject.com/en/dev/intro/tutorial02/)
* [Writing your first Django app, part 3](https://docs.djangoproject.com/en/dev/intro/tutorial03/)
* [Writing your first Django app, part 4](https://docs.djangoproject.com/en/dev/intro/tutorial04/)
* [Writing your first Django app, part 5](https://docs.djangoproject.com/en/dev/intro/tutorial05/)
