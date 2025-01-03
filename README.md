# local_munki_repo_test
Automate the configuration of a local munki repo needed for an munki application install testing workflow using VM's


### The following script must be run before the first use of the workflow:
* The <ins>**start_setup.sh**</ins> script configures the directory structure in _/Users/Shared/_, as shown in the following wiki: [Demonstration Setup - Munki Client Configuration](https://github.com/munki/munki/wiki/Demonstration-Setup#munki-client-configuration).


### The following two scripts are required to use the local Munki workflow:
* The <ins>**setup_server.sh**</ins> script starts the server and configures the Munki repo preferences to use the created directory structure. Using UTM, a prepared template VM (which must be created and configured in advance) is duplicated and launched. A connection to the copied VM is established via screen sharing.
* The <ins>**revert_changes.sh**</ins> script should be run after finishing with the workflow to stop the server and revert to the default Munki repo preferences.


### The template VM must be configured as follows:
* The name, language, and password of the template must match the variables in the *setup_server.sh* script.
* Configure the shared directory in UTM (under "New Shared Directory") to connect the local _/Users/Shared/munki_repo_ directory with the VM.
* Enable screen sharing at _System Settings > General > Sharing > Screen Sharing_ to ensure a working VNC connection.
* Install the latest version of Munki: [Download Munki](https://github.com/munki/munki/releases)
* Copy the serial number of the VM to create a Manifest
* Configure your SoftwareRepoURL using following command: `sudo defaults write /Library/Preferences/ManagedInstalls.plist SoftwareRepoURL “file:///Volumes/My Shared Files/munki_repo”`


### How to use the workflow:
* Create a new Manifest in MunkiAdmin (in the _/Users/Shared/munki_repo_ repository) using the VMs serial number as name and activate the testing catalog. (You can skip this step if you already used the workflow with the same template.)
*  Run a munki recipe using autopkg on your host. 
* Now add the software as *optional_install* in MunkiAdmin in the VMs manifest.
* In your VM you can run a managedsoftwareupdate. 
* You should be able to install the software in the Managed Software Center.

