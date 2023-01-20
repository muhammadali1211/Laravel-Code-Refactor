Do at least ONE of the following tasks: refactor is mandatory. Write tests is optional, will be good bonus to see it. 
Please do not invest more than 2-4 hours on this.
Upload your results to a Github repo, for easier sharing and reviewing.

Thank you and good luck!



Code to refactor
=================
1) app/Http/Controllers/BookingController.php
2) app/Repository/BookingRepository.php

Code to write tests (optional)
=====================
3) App/Helpers/TeHelper.php method willExpireAt
4) App/Repository/UserRepository.php, method createOrUpdate


----------------------------

What I expect in your repo:

X. A readme with:   Your thoughts about the code. What makes it amazing code. Or what makes it ok code. Or what makes it terrible code. How would you have done it. Thoughts on formatting, structure, logic.. The more details that you can provide about the code (what's terrible about it or/and what is good about it) the easier for us to assess your coding style, mentality etc

And 

Y.  Refactor it if you feel it needs refactoring. The more love you put into it. The easier for us to asses your thoughts, code principles etc


IMPORTANT: Make two commits. First commit with original code. Second with your refactor so we can easily trace changes. 


NB: you do not need to set up the code on local and make the web app run. It will not run as its not a complete web app. This is purely to assess you thoughts about code, formatting, logic etc


===== So expected output is a GitHub link with either =====

1. Readme described above (point X above) + refactored code 
OR
2. Readme described above (point X above) + refactored core + a unit test of the code that we have sent

Thank you!

Code to refactor
=================
1) app/Http/Controllers/BookingController.php
a)  As in the Controller you have inject the Repository in __construct function that use the service container concept and i think its good approach instead
    of calling BookingRepository everytime like with new keyword new BookingRepository(); it will load the memory everytime. despite this few more things
    that should be in our Controller function.
    1) We should use Request validation class to validate the post request like artisan make:request and use in the function like update(UpdateRequest $request).
    2) in distanceFeed function instead of $request->all() should use request->get('distance') and all conditional code should be in seprate function/file.
    3) Model should load from seprate Repository like DistanceRepositry, JobRepositry and these should be inject in the __construct.
    4) Each Repository/Service should have basic resource functions index, store, update, show, destroy.
    5) Docs type comments is also ok
2) app/Repository/BookingRepository.php
   In BookingRepository you have inject Job Model in __construct function that is good approach.
   like this use $this->model = $model; instead of Job::method 
   1) in BookingRepository should follow below approach for Job and User model
      function __construct(Job $job, MailerInterface $mailer, User $user, Language $language){
        $this->mailer =$mailer;
        $this->job = $job;
        $this->user = $user;
        $this->language = $language;
      }
      and use in other functions like $this->user->store(data);
         $user = $job->user()->get()->first(); instead user only $job->user()->first()
         use Carbon instead of new date
    2) use DB Transcation in try catch to handle Exception try block DB::beginTransaction();   === DB::commit() and catch block DB::rollback()
    3) use constant value instead https://onesignal.com/api/v1/notifications
    4)  $mailer = new AppMailer(); avoid using new use always __construct approach this keyword.
    5) instead multiple where in query use whereHas
    6) store function i would recommend the switch over if-else

    Note: I have tried to figure out most of the things hopefully I WILL lead us toward better approach and code formatting.