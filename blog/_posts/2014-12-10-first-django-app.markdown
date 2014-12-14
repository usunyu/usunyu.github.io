---
layout: post
title: Writing your first Django app
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
[]
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save()
>>> q.id
1
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)
>>> q.question_text = "What's up?"
>>> q.save()
>>> Question.objects.all()
[<Question: Question object>]
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
[<Question: What's up?>]
>>> Question.objects.filter(id=1)
[<Question: What's up?>]
>>> Question.objects.filter(question_text__startswith='What')
[<Question: What's up?>]
>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>
>>> Question.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Question matching query does not exist.
>>> Question.objects.get(pk=1)
<Question: What's up?>
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True
>>> q = Question.objects.get(pk=1)
>>> q.choice_set.all()
[]
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)
>>> c.question
<Question: What's up?>
>>> q.choice_set.all()
[<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]
>>> q.choice_set.count()
3
>>> Choice.objects.filter(question__pub_date__year=current_year)
[<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
{% endhighlight %}

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

### Make the poll app modifiable in the admin:
`polls/admin.py`:
{% highlight python %}
from django.contrib import admin
from polls.models import Question

admin.site.register(Question)
{% endhighlight %}

### Customize the admin form:
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

### Adding related objects:
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

### Customize the admin change list:
`polls/admin.py`:
{% highlight python %}
class QuestionAdmin(admin.ModelAdmin):
	# ...
	list_display = ('question_text', 'pub_date', 'was_published_recently')
	list_filter = ['pub_date']
	search_fields = ['question_text']
{% endhighlight %}

### Customize the admin look and feel:
`mysite/settings.py`
{% highlight python %}
TEMPLATE_DIRS = [os.path.join(BASE_DIR, 'templates')]
{% endhighlight %}


#### Resource:
* [Writing your first Django app, part 1](https://docs.djangoproject.com/en/dev/intro/tutorial01/)
* [Writing your first Django app, part 2](https://docs.djangoproject.com/en/dev/intro/tutorial02/)
