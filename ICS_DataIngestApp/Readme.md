Steps to Build and Deploy Application on Bluemix
-------------------------------------------------
1. Import the code in Eclipse with WebSphere Liberty Server running locally.
2. Modify and update Server.xml of existing liberty with below entries:
      <library id="kafkaLoginLib">
          <fileset dir="${shared.resource.dir}/kafkalibs" includes="messagehub.login-*.jar"/>
      </library>
      
     <jaasLoginModule className="com.ibm.messagehub.login.MessageHubLoginModule" id="KafkaClient" libraryRef="kafkaLoginLib">
    	  <options password="vAXdqbmokRDlG9QitbwA4ddOquRkSmej" serviceName="kafka" username="br1XTccWWjOvzxiv"/>
     </jaasLoginModule>
     <jaasLoginContextEntry id="KafkaClient" loginModuleRef="KafkaClient" name="KafkaClient"/>
     
     Where username=<Username of existing Message Hub Instance>
           password=<Password of existing Message Hub Instance>
           
     Copy folder containing message hub dependencies into Liberty server resource directory before making above reference in server.xml.
     
  3. Export the server configuration by running below command or from eclipse by rt click on liberty server
               wlp/bin/server package defaultServer --include=usr
               
  4. Deploy the application on Bluemix as "Liberty for Java" app using below command.
               bluemix cf push <yourappname> -p wlp/usr/servers/defaultServer/defaultServer.zip
               
     Refer below link for more details (Section: Packaged Server):
     
     https://console.ng.bluemix.net/docs/runtimes/liberty/optionsForPushing.html
               
  5. Bind the deployed application with existing message hub service instance and restage the application.
  
  6. Triggering the application
     ---------------------------
   
   To trigger the application use the below URL and details:
   
Endpoint URL:
https://<appURL>/DataInjestAppWeb/rest/testservice/fetchAssetData?cat=prod&load=product&batchsize=40

Attributes:

 i) cat
       value        topic
    a) prod        inventory
    b) wh            facility
    c) vend        openorder
    d) fac            shipment

ii) load
           value        path
        a) product        /home/nprathap/ICSLZ/Product_Master.csv
        b) warehouse    /home/nprathap/ICSLZ/Load_Warehouse_Master1.csv
        c) vendor        /home/nprathap/ICSLZ/Load_VendorMaster3.csv
        d) facility        /home/nprathap/ICSLZ/Load_Facility_Master1.csv

apiKey: <copy apiKey from binded message hub reference to application>
user: <copy username from binded message hub reference to application>
password: <copy password from binded message hub reference to application>

Copy broker URL reference from binded message hub reference to application example -
  "kafka_brokers_sasl": [
          "kafka01-prod02.messagehub.services.eu-gb.bluemix.net:9093",
          "kafka02-prod02.messagehub.services.eu-gb.bluemix.net:9093",
          "kafka03-prod02.messagehub.services.eu-gb.bluemix.net:9093",
          "kafka04-prod02.messagehub.services.eu-gb.bluemix.net:9093",
          "kafka05-prod02.messagehub.services.eu-gb.bluemix.net:9093"
        ]

To test locally from Eclipse refer to my earlier mail with REST Client
  
  