# Web Preview

The Cloud Shell Web Preview is a quick and easy way to test web services without leaving your browser. 

Web Preview creates a publicly available proxy on your specified port, allowing to access any web service running on that port. The proxy is secured over HTTPS and includes an authentication layer to ensure that only you can access your exposed service. Web Preview runs on port 8080 by default, but can be mapped to any port from 8080 - 8084 via the Web Preview settings.

To start the Web Preview, click Web Preview -> Preview on port 8080. 

This will create the proxy and open a new browser tab to the public proxy URL. To change the exposed port, click Web Preview | Change Port and choose your desired port, or simply modify the port number at the start of the proxy URL.

# The first app

Google offers integrations for publicly hosted git repositories on GitHub and BitBucket. With the click of a button, one can clone git repositories into their Cloud Shell $HOME directory and then open them in a new Cloud Shell Code Editor session.

1 Visit the git repository (https://github.com/akalu/gcp-grimoire) and click Open in Cloud Shell.

2 In the shell window, go to the Cloud Shell demo directory with cd code.

3 Install node dependencies with ```npm install```

4 Start the sample app with ```npm start```

5 View the running application in your browser by clicking Web Preview.

# Notes

While many tools and languages are included out of the box, there may be times you need to bring additional tools into your Cloud Shell VM. 
Because everything outside of the $HOME directory will be lost after a short period of inactivity, installing new tools outside of $HOME is not a way





