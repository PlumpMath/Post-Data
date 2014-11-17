Post-Data
=========

Custom Resource Interceptor for Awesomium embedded browser

Author:     Garison E Piatt
Contact:    web@garisonwebdesign.com
Created:    11/17/14
Version:    1.0.0

Apparently, when Awesomium was first created, the programmers did not understand that someone would
eventually want to post data from the application.  So they made it incredibly difficult to upload
POST parameters to the remote web site.  We have to jump through hoops to get that done.

This module provides that hoop-jumping in a simple-to-understand fashion.  We hope.  It overrides
the current resource interceptor (if any), replacing both the OnRequest and OnFilterNavigation
methods (we aren't using the latter yet).

It also provides settable parameters.  Once this module is attached to the WebCore, it is *always*
attached; therefore, we can simply change the parameters before posting to the web site.

File uploads are currently unhandled, and, once handled, will probably only upload one file.  We
will deal with that issue later.

To incoroprate this into your application, follow these steps:
 1.  Add this file to your project.  You know how to do that.
 2.  Edit your MainWindow.cs file.
     a.  At the top, add:
             using CRI;
     b.  inside the main class declaration, near the top, add:
             private CustomResourceInterceptor cri;
     c.  In the MainWindow method, add:
             WebCore.Started += OnWebCoreOnStarted;
             cri = new CustomResourceInterceptor();
         and (do this *before* you set the Source value for the Web Control):
             cri.Enabled = true;
             cri.Parameters = String.Format("login={0}&password={1}", login, pw);
         (Choose your own parameters, but format them like a GET query.)
     d.  Add the following method:
             private void OnWebCoreOnStarted(object sender, CoreStartEventArgs coreStartEventArgs) {
                 WebCore.ResourceInterceptor = cri;
             }
 3. Compile your application.  It should work.
