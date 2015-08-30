---
layout: post
title:  "Stop that Annoying Firefox Sound"
date:   2015-08-30 11:43:17 +0530
categories: firefox sounds
---

So Chrome doesn't want to [kill memory on your computer][chrome-memory-leak];
it just wants to hurt it very, very badly. So, i('ve always) use(d) Firefox. But
there is that annoying sound every time you try to close the window. It comes
with this dialogue:

![Annoying Firefox dialogue](/assets/images/2015/08/30/Screenshot 2015-08-30 11:50:01.png)

And if you're listening to some soothing music on YouTube and you've just
called it a day, closing Firefox will ensure that you can't sleep at least for
the next 20 minutes.


#### You could just turn off notifications

The simplest solution is to go nuclear: just don't use Firefox in the first
place. Or just turn off your speakers.

OK, you use it with your speakers on. And everything else works perfectly for
you. Just that Firefox, in its final moments of survival, wants to plead for
mercy and live a little longer. So you could simply turn off only that
notification.

![Stop Firefox from bothering you when you want to close it](/assets/images/2015/08/30/Screenshot 2015-08-30 12:00:11.png)

#### Only turn off the sound

Well, you're a neo-robots-rights-activist, and you want to give the freedom to
speech even to computer apps. You want to retain Firefox's cry for mercy, but
you just can't bear to hear the anguish.

So you go to the censor board to filter such noises from reaching you. Ubuntu is
the ruling government on your computer, but its Sound Settings bureaucracy is a
bit atomic: either you get all the gore, or none at all.

So you infiltrate into the government files (with your all powerful root access)
and go [straight to the source of the problem][ubuntu-sound-files]:
`/usr/share/sounds/ubuntu/stereo/dialog-question.ogg`

You rename the file to some other thing, so that the Sound bureaucracy can't
find it later. Just be sure to handle `system-ready.ogg` which points to the
`dialog-question.ogg` file. You don't want the government to get too suspicious.

![Culprit sound files](/assets/images/2015/08/30/Screenshot 2015-08-30 12:16:13.png)

Once done, enjoy the peace and serenity of your palace, while the robot
(slave) Firefox app cries in anguish. Don't worry, you won't be able to hear
it!~





​

Thank you for scrolling to the bottom of this page! Do let me know
how it was.

*******

[chrome-memory-leak]: https://productforums.google.com/forum/#!msg/chrome/Gaq6YhWO2SY/Bm5RYw9ORk4J
[ubuntu-sound-files]: https://ubuntuforums.org/showthread.php?t=1312356&p=8370344&mode=linear#post8370344
