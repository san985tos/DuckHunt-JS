# DUCK HUNT JS v3.0

[Play the game](http://duckhuntjs.com)

This is an implementation of DuckHunt in Javascript and HTML5. It uses the PixiJS rendering engine, Green Sock Animations, Howler, and Bluebird Promises.

## Rendering
This game supports WebGL and Canvas rendering via the PixiJS rendering engine.

## Audio
This game will attempt to use the WebAudioAPI and fallback to HTML5 Audio if necessary. Audio is loaded and controlled via HowlerJS.

## Tweening
The animations in this game are a combination of PixiJS MovieClips built from sprite images and tweens. Since PixiJS doesn't provide a tweening API, Green Sock was used.

## Game Logic
The flow of this game is managed using Javascript. The main chunks of business logic are implemented as ES6 classes which are transpiled to ES5 using Babel.

## Working With This Repo

 - You must have [nodejs](https://nodejs.org/) installed.
 - Clone the repo into a directory of your choice
 - `cd` into that directory and run `npm install`
 - Use `npm start` to start a local webserver which will make the site available at http://localhost:8080/. Cross origin errors prevent this project from being accessed in the browser with the `file://` protocol. This will also trigger automatic builds and reloads of the page when changes are detected in the `src` directory.
 - If you want to manually cut a build of the application code run `npm run build`
 - If you want to rebuild audio assets use `npm run audio`
 - If you want to rebuild image assets use `npm run images`

## Bugs
Please report bugs as [issues](https://github.com/MattSurabian/DuckHunt-JS/issues).

## Contributing
Pull requests are welcome! Please ensure code style and quality compliance with `npm run lint` and include any built files.


## OpenShift guide
 - Create a new project
 ```
 oc new-project test
 ```
 - Now execute following command to deploy example application:
 ```
 oc new-app nodejs~https://github.com/vrutkovs/DuckHunt-JS
 ```
 - This will create all Kubernetes resources to deploy and run the example application as below:
 ```
 --> Creating resources ...
    imagestream.image.openshift.io "duckhunt-js" created
    buildconfig.build.openshift.io "duckhunt-js" created
    deployment.apps "duckhunt-js" created
    service "duckhunt-js" created
--> Success
    Build scheduled, use 'oc logs -f buildconfig/duckhunt-js' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/duckhunt-js'
    Run 'oc status' to view your app.
```
- Our application will now build from source, you can watch it happen by tailing the build log file. When it's finished it will push the image into the OpenShift image registry:
```
oc logs duckhunt-js-1-build -f
```
- This process will likely take approximately 3-4 minutes for the following output to confirm the new image is pushed to registry:
```
Successfully pushed image-registry.openshift-image-registry.svc:5000/test/duckhunt-js@sha256:c4e64bc633ae09ce0f2f2f6de2ca9eaca8e11dc5b335301a2be78216df4b6929
Push successful
```
- Once finished, check the status of the pods by executing the command below:
```
oc get pods 
```
- You'll see that a couple of pods have been created, one that just completed our build, and then the application itself, which should be in a Running state, if it's still showing as ContainerCreating just give it a few more seconds:
```
NAME                           READY   STATUS      RESTARTS   AGE
duckhunt-js-1-build            0/1     Completed   0          4m7s
duckhunt-js-5b75fd5ccf-j7lqj   1/1     Running     0          105s   <-- this is our app!
```
- Now expose the application (via the service) so we can route to it from the outside...
```
oc expose svc/duckhunt-js
```
- As a result, a route is created:
```
route.route.openshift.io/duckhunt-js exposed
```
- To check the route execute following command:
```
oc get route duckhunt-js
```
- Now you should be able to see the route endpoint as below:
```
NAME          HOST/PORT                                  PATH   SERVICES      PORT       TERMINATION   WILDCARD
duckhunt-js   duckhunt-js-test.apps.k2s66.dynamic.opentlc.com          duckhunt-js   8080-tcp                 None
```
- You should be able to open up the application in the same browser that you're reading this guide from, either copy and paste the address.