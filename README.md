Save your application from being captured by OS in background or in-active state.

You might have noticed that in applications switcher mode, all running applications screens are displayed. They are actually screenshots of applications that are used for animation purposes by OS.

When and why are these screenshots taken?
When the home key on an iPhone or iPad is pressed, a screenshot is immediately taken of the current application. This is done to generate an animation of the application which appears to “shrink” into the screen. The image is also stored for use as a thumbnail image for the running application. 

Problem due to these screenshots?
If sensitive information was displayed in your application at the time of the screenshot, serious security implications may arise. Personal information may unknowingly be leaked and used for or unwanted purpose.

For the proof of this concept. Run different applications on your iOS device. Press home to enable application’s switcher mode. You will be able to see applications' screens with a clear display of their content in it. Attached is a screenshot of my phone with different games and apps running.

 

Can we protect application from it?
The answer to this question is YES. You can protect your application from getting a screenshot of your sensitive data. We will show you a simple way to do it.
Getting Started
To build a sample for Protect app. You need to follow bellow steps.
1.    Xcode project creation
2.    Addition of some content on main screen
3.    Addition for protection view for app, when it switch to background state
4.    Removal of protection view, with app switch back to active state


1. XCode Project Creation

Open up XCode and create a single view new project “ProtectApp”. User Storyboard to design application UI. Select “Swift” from application language option. 

2. Addition of some content on main screen

Add some content on view controller. So that you can easily identify application in switcher mode and also can distinguish it between your protection view and main app.


3. Addition for protection view for app, when it switch to background state

While application is about to switch from active state to background or inactive state, add a custom view as an overlay to your application full screen. So, when OS tries to take a screenshot of it, it will display your custom view in application switcher mode.

Let's check it out how to do that.

Add below methods in your AppDelegate class.

//to add protection to your app content
func protectAppContentFromScreenshot ()
    {
        // fill screen with our own colour
        let securityView = UIView.init(frame: self.window!.frame)
        securityView.backgroundColor = UIColor.red
        securityView.tag = 22
        securityView.alpha = 0;
        self.window?.addSubview(securityView)
        self.window?.bringSubviewToFront(securityView)
        
        // fade in the view
        UIView.animate(withDuration: 0.2) {
            //
            securityView.alpha = 1
        }
    }


Now call protectAppContentFromScreenshot() from applicationWillResignActive 
Method of AppDelegate class.



4. Removal of protection view, with app switch back to active state

When application switch back to active state from background or inactive one, you will need to remove added protection view from application view.

For this you will need to add removeProtectionFromApp method from applicationDidBecomeActive Method of AppDelegate class.

//remove protection from your app content
func removeProtectionFromApp ()
    {
        let securityView = self.window?.viewWithTag(22)
        UIView.animate(withDuration: 0.5, animations: {
            //
            securityView?.alpha = 0
        }) { (completed) in
            //
            securityView?.removeFromSuperview()
        }
    }
