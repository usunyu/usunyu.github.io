---
layout: post
title: Fix permission denied (publickey) when SSH Access to Amazon EC2
---

I accidentally messed up the `/home/ubuntu/.ssh/authorized_keys` of AWS EC2, and cannot access EC2 from SSH. Here are the steps for fixing it:

1. Detach the bad EBS volume.

2. Create a new EC2 instance, and attach that bad EBS volume to it.

3. Mount that bad EBS volume from the new created EC2 instance.
```
sudo mount /dev/xvdb1 /vol
```

4. Fix the  `/home/ubuntu/.ssh/authorized_keys`, and umount and detach the EBS volume.
```
sudo umount /vol
```

5. Attach the fixed EBS volume to root device `/dev/sda1` of the origin EC2 instance.

#### Reference:
* [Making an Amazon EBS Volume Available for Use on Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)
* [how change root device on instance](https://forums.aws.amazon.com/thread.jspa?threadID=83756)