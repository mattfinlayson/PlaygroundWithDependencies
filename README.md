Recently, I have been teaching myself swift and exploring 3rd party libraries. It seemed like playgrounds would be the natural way to do this, but I have spent hours trying to figure out how to make it work. My breakthrough finally came when I found [this stackoverflow answer explaining the requirements](http://stackoverflow.com/questions/24046160/how-to-i-import-3rd-party-frameworks-into-xcode-playground). Of course, the explanation outlined what needed to happen to use external frameworks but didn't explain how to make it work. With an evening of project creation I was finally able to get something in a working state using cocoapods. This post assumes you have used cocoapods and have it installed in order to proceed.

In XCode do the following:

```
File -> New -> Project
Single View -> Next
Name: ProjectWithDependencies
Choose a location and click create.
```

Now run the project. I DON'T KNOW WHY, but without this step the workspace is not correctly configured.

```
File -> File -> iOS -> Playground
Name: PlaygroundWithDependencies
The default location and options are fine, click create
```

You now have a project, containing a generic single view application. You also have a playground which says "hello world".

Now close XCode, open your terminal, and cd to the ProjectWithDependencies directory. For this post we'll be pulling in Alamofire and Argo to our playground. To do this we will create a cocoapods project.

```
pod init // create a Podfile
```

Now edit the Podfile to include our dependencies.
```
# Uncomment this line to define a global platform for your project
# platform :ios, '6.0'

use_frameworks!

pod 'Alamofire'
pod 'Argo'

target 'PlaygroundWithDependencies' do

end

target 'PlaygroundWithDependenciesTests' do

end
```

Finally (make sure XCode is closed) and run:
```
pod install
open ProjectWithDependencies.xcworkspace
```

The last command line step is to actually run pod install to generate the workspace file and pull down the dependencies we just specified.

When we open the workspace the natural thought is to open the Playground and import the new libraries, however, if you do this you'll see that the project does not recognize them. The final prerequisite is to build the new frameworks. Click and hold on the scheme for 'ProjectWithDependencies' in XCode and select 'Manage Schemes...'

From there select the check boxes for the Pods we will want to import. For each one choose the scheme and build with the run button or with CMD+R. **You must do this for each Pod you with to import.** In this case Pods-Alamofire and Pods-Argo

Finally open the playground file itself you can now import Argo and Alamofire without error.
