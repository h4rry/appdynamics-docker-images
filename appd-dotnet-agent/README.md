### .Net Core Agent Image

* Download the latest agent bundle from https://download.appdynamics.com/download/#version=&apm=dotnet%2Cdotnet-core&os=&platform_admin_os=&appdynamics_cluster_os=&events=&eum=&page=1

* Make a note of sha256 checksum value and the full version of the agent (e.g. 4.5.8.0)

* Run `./build.sh` passing the version and the checksum. The checksum can be left blank to avoid validation. 
It is a best practice to validate the package integrity.


An example yaml spec is provided to demonstrate how to deploy the AppDynamics .Net Core agent in the init container and pass it to the application to be instrumented. 
It is located in the `deploy` folder along with a template confgMap with required environment variables.