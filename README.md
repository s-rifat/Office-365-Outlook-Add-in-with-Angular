# How to develop Office 365 - Outlook Add-in with Angular

Assuming that the latest Node.js and Git are already installed.

**Create an Outlook Add-in Project** \
Install the office Yeoman generator : `npm install -g yo generator-office` \
Generate the office adding project with command: `yo office`

* _Choose a project type:_
    Office Add-in Task Pane project using Angular framework

 * _Choose a script type:_ 
     Typescript
    
 * _What do you want to name your add-in (My Office Add-in):_
   testAddin

 * _Which Office client application would you like to support:_ 
   Outlook


**Create an Angular Project**\
Install the Angular CLI: `npm install -g @angular/cli`\
Generate an angular app: `ng new testAngular`\
Change directory to the root of Angular project: `cd testAngular`

**Transfer Files from testAddin to testAngular**
* (Optional) Copy all the images from the _assets_ folder of _testAddin_ to _src/assets_ folder of _testAngular_
* Copy the _manifest.xml_ from _testAddin_ and paste it in the root folder of _testAngular_

**Make changes in Manifest file**
* Find _taskpane.html_ in the manifest file and replace it with _index.html_
* Find https://localhost:3000 in the manifest file and replace it with https://localhost:4200. The URL should start with _https_ not _http_.

**Install dependencies** \
`npm install --save @microsoft/office-js` \
`npm install --save @microsoft/office-js-helpers` \
`npm install --save office-ui-fabric-js`

**Install dev dependencies** \
`npm install --save-dev @types/office-js`\
`npm install --save-dev @types/office-runtime`\
`npm install --save-dev office-addin-debugging`\
`npm install --save-dev office-addin-dev-certs`\
`npm install --save-dev office-toolbox`

**Update the tsconfig.app.json**
* Add _office-js_ under the types array under _compilerOptions_ - _"types": [ "office-js"]_ 
* Add the new _target_ under _compilerOptions_ - _"target": "es5"_

![image](https://user-images.githubusercontent.com/47311938/219958768-4bd4a762-1706-47f1-b50e-93dd03552907.png)

**Update index.html**\
`<script>
    let pushStateRef = history.pushState;
    let replaceStateRef = history.replaceState; </script>`
  
`<script type="text/javascript" src="https://appsforoffice.microsoft.com/lib/1.1/hosted/office.js"></script>` 

`<link rel="stylesheet" href="https://static2.sharepointonline.com/files/fabric/office-ui-fabric-js/1.4.0/css/fabric.min.css" />` 

`<link rel="stylesheet" href="https://static2.sharepointonline.com/files/fabric/office-ui-fabric-js/1.4.0/css/fabric.components.min.css" />` 

`<script src="https://static2.sharepointonline.com/files/fabric/office-ui-fabric-js/1.4.0/js/fabric.min.js"></script>`

`<script>
    history.pushState = pushStateRef;
    history.replaceState = replaceStateRef;
    delete pushStateRef;
    delete replaceStateRef;</script>`

![image](https://github.com/s-rifat/Office-365-Outlook-Add-in-with-Angular/assets/47311938/5df36c8e-d953-446d-894e-7c9c137d9952)

**Update main.ts** \
`Office.initialize = function () {
  platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.error(err));
}`

![image](https://user-images.githubusercontent.com/47311938/219958850-8ccb525d-4b11-4007-b5b3-98db3b3224d3.png)

**Update app-routing.module.ts**
* Add _{ useHash: true }_ in imports array

![image](https://user-images.githubusercontent.com/47311938/219958908-39d3640a-7c23-4068-9766-b5aa5aec15bf.png)

**Run the angular project over https**
*	Open cmd
*	Type the following three commands:\
	`git clone https://github.com/s-rifat/generate-trusted-ssl-certificate.git`\
	`cd generate-trusted-ssl-certificate`\
	`bash generate.sh`
*	Go to _C/Users/{your-name}/generate-trusted-ssl-certificate_ folder (the location where you have executed the git commands)
*	Double click on the certificate _(server.crt)_
*	Click on the button _Install Certificate …_
*	Select _Local Machine_
*	Click _Next_
*	Select _Place all certificates in the following store_
*	Click _Browse_
*	Select _Trusted Root Certification Authorities_
*	Click _Ok_
*	Click _Next_
*	Click _Finish_
*	If you get a prompt, click _Yes_
*	Create a folder named _ssl_ in the _testAngular_ project folder.
*	Copy the _private key_ and _root certificate_ to the _ssl_ folder

![image](https://user-images.githubusercontent.com/47311938/219958652-21636754-836d-48b1-bef0-fba10d59c6bb.png)

* In _package.json_ file add `"start-ssl": "ng serve --ssl true --ssl-cert \"ssl/server.crt\" --ssl-key \"ssl/server.key\""` under scripts.

![image](https://user-images.githubusercontent.com/47311938/219960773-8ac6f7f8-59a8-44d5-8208-f77c9cc3111f.png)

* Use this command `npm run start-ssl`, it will run the angular project in https://localhost:4200


**Side Load Outlook Add-in**
*	Go to https://outlook.live.com/ and sign in with your onmicrosoft account \
*	Select any message from inbox and click the three dots in the upper-right corner of the message

![image](https://user-images.githubusercontent.com/47311938/219959232-245b4c3a-423a-4af5-911a-fa9825989219.png)


* Click Get-Add-ins 


![image](https://user-images.githubusercontent.com/47311938/219959260-46f88c5c-4be1-4c88-a1e6-1ea6e7f47803.png)


* Select _My add-ins_, then click _Add a custom add-in_, after that click _Add from file_


![image](https://user-images.githubusercontent.com/47311938/219960879-5ffd22e7-c624-4396-ac4d-1bbbb5d456dd.png)



*	Open the manifest file



![image](https://user-images.githubusercontent.com/47311938/219959673-93445989-479a-491d-8c8e-4dbf029ab30e.png)


* 	Click install


![image](https://user-images.githubusercontent.com/47311938/219959709-0da6f988-aaa2-4bf4-b2f4-a68ddda66a6d.png)


* Open a message, click the three dots, click testAddin, click Show Taskpane


![image](https://user-images.githubusercontent.com/47311938/219962363-abf0d9af-b534-4b27-8789-842b7dbb4012.png)



* If everything works fine, we will see our Outlook Add In with default Angular Template.


![image](https://user-images.githubusercontent.com/47311938/219959771-1e55ca3f-05d4-492e-8b21-8c62a0e0bc4a.png)


* We can access outlook object using: `Office.context.mailbox.item` 


![image](https://user-images.githubusercontent.com/47311938/219960987-4fbf44a5-bb05-4d06-9e98-b0b0fb5443b3.png)

[Video Tutorial](https://youtu.be/2LoSHS5mpCY)

[Official Documentation](https://learn.microsoft.com/en-us/office/dev/add-ins/outlook/)

 **Special credit to [initgrep](https://www.initgrep.com/posts/javascript/angular/microsoft-office-addin-using-angular-cli)
 and [Ragavan Rajan](https://ragavanrajan.medium.com/building-office-add-in-using-angular-8-209624ba61ed) for their blogs.**



















