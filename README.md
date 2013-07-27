# [Travis CI](http://travis-ci.org) integration with [OpenShift](http://openshift.com)
This is a sample OpenShift application that illustrates integration
with open source continuous integration system, Travis CI.

# How to Use
This is a Ruby 1.9 application.
Using the `rhc` utility, create an application:

```
rhc app create myapp ruby-1.9 --from-code https://github.com/travis-ci/openshift-travis-quickstart.git
```

Or, you can use the [Console](https://openshift.redhat.com/app/console/application_types/quickstart!15074).

## Add Github remote
`rhc app create` creates a local working copy in `myapp`.

We will work in that copy: `cd myapp`

This copy has a git remote pointing to your OpenShift gear,
so it is a good idea to rename it before
going further:

```
git remote rename origin openshift
```

Next, create a new repository on [Github](https://github.com/new).
The name can be anything, but for the purpose of illustration,
we call it `myapp`.

Finally, add a remote pointing to Github:

```
git remote add origin https://github.com/<USER>/myapp.git
```
And push code:

```
git push -u origin master
```

## Enable Travis on the Github repo
Next, visit [Travis profile page](https://travis-ci.org/profile) and locate
your new application `myapp` and enable Travis integration.

If you do not see it, press "Sync now" button near the top of the page.

## Edit `.travis.yml`
This quickstart has a near-empty `.travis.yml`
(more on this [below](#note-on-build-script)).
After running `bundle install` to install dependencies, run `travis`
to populate the `deploy` section:

```
$ travis setup openshift
OpenShift user: user@example.com
OpenShift password: ************
OpenShift application name: |myapp|
OpenShift domain: openshiftdomain
Deploy only from <USER>/myapp? |yes| 
Encrypt password? |yes| 
```

### Note on Build Script
The default build script for a Ruby application on Travis is
`rake`.
This is not applicable for this simple application, so we override this
with a simple `true` script that succeeds every time.

## Deploy!
Finally, it is time to deploy to Github.
This will trigger a build on Travis, which in turn
pushes code to your OpenShift gear on your behalf using the credentials
given in `.travis.yml`.

```
git add .travis.yml
git commit -m 'Set up Travis build'
git push origin master
```

## Check the Result
Since the deployment to your OpenShift gear will be initiated by
Travis, you will not see the deployment result from the `git push`
above.
You can check the status with [Travis](http://travis-ci.org), or
you can run:

```
travis logs -r <USER>/myapp
```
