The application is usefull if you have off hours support who need to reset user passwords on 3rd shift, weekend and holidays (very usefull).
PURPOSE: Reset Avatar User Passwords to a temporary password
HOW: The application calls Netsmart web services: WEBSVC.UserManagement web services for methods such as: DoesUserExist or GeneratePassword.
ENVIRONMENT: Netsmart Avatar 2017
Note, the 2017 is important.  This version of the Cache database requires wsse (Web Services Security Enhanced) headers.  The header is set within the SecurityHeader.cs file.
Prior to 2017 (e.g. Cache 2010) the wsse security header was not required.  CSP is the Cache Server Pages gateway, avpm is Avatar PM, cls is Class file, WSDL is the web 
service definition language xml file.  You will see references to these in the urls below.
Finally, the connected services can be generated directly from Microsoft Visual Studio (I use Visual Studio community edition 2019 version: 16.6.2, anything beyond 10.0 is fine ?). 
Open the solution explorer, select the csproj file (below the solution file), right click select Add\service reference\enter the web service url in the address bar 
(similar to the following: http://$YOURERVERNAME:8972/csp/AVPM/WEBSVC.UserManagement.cls?WSDL=1), rename the name space appropriately, and select GO.  This will 
generate all of the connected services files.  Note as below, replace $YOURSERVERNAME WITH the name of your Avatar instance.
Good luck, it's straight forward if you follow all steps above and below.
two quick updates to program.cs:
//  Update 1: 8/13/2020 prompt the help desk user to enter the help desk password
//  Update 2: 8/14/2020 Mask the Help desk user's reponse.

________________________________________________________
Author: Chris Kennedy
cgkboston@gmail.com
ckennedy@aspirehealthalliance.org
339.201.0243
________________________________________________________
Important information below:
========================================================

Set the following varaiables in AVATOOL\AVATOOL\program.cs
String AvatarEnv = "LIVE";  // LIVE, SBOX or UAT.
Boolean UserExistsP = false; // init to false.
String HelpDeskUser = "HELPDESK"; // a user on the server with access to the AVPM Quick User Update form only.
String userid = ""; // Always initialize the user id to null.
// The user name token appears to be any number you want it to be ???, ..., I choose 250.
String UserSecurityToken = "UsernameToken - 250";

Update ...\AVATOOL\AVATOOL\App.Config replace $YOURAVATARSERVER WITH either your sandbox, uat or live instance.

Update the following Connected services files to match your environment:
Note replace $YOURAVATARSERVER WITH either your sandbox, uat or live instance.
Likely your port is the same (8972).
The folder is something like ...\AVATOOL\AVATOOL\Connected Services\User Management:

Note the key here is to update the app.config for your environment.  Once you create the connected services it should add them with your avatar url.
To do this open visual studio and select the project, add service reference.  This will create the AVATOOL\Connected Services\UserManagement folder and its contents.
The key here is to know the url to put in the address.  It should be something like: 
http://$YourAvatarServer:8972/csp/avpm/WEBSVC.UserManagement.cls?WSDL=1


C:\Z_AVATOOL_Template\AVATOOL\Connected Services\UserManagement\configuration.svcinfo
    <endpoint normalizedDigest="&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-16&quot;?&gt;&lt;Data 
address=&quot;http://$YOURAVATARSERVER:8972/csp/avpm/WEBSVC.UserManagement.cls&quot; binding=&quot;basicHttpBinding&quot;
	bindingConfiguration=&quot;WebServicesSoap&quot; contract=&quot;UserManagement.WebServicesSoap&quot; 
	name=&quot;WebServicesSoap&quot; /&gt;" digest="&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-16&quot;?&gt;&lt;Data
address=&quot;http://$YOURAVATARSERVER:8972/csp/avpm/WEBSVC.UserManagement.cls&quot; binding=&quot;basicHttpBinding&quot; 
	bindingConfiguration=&quot;WebServicesSoap&quot; contract=&quot;UserManagement.WebServicesSoap&quot;
	name=&quot;WebServicesSoap&quot; /&gt;" contractName="UserManagement.WebServicesSoap" name="WebServicesSoap" />
C:\Z_AVATOOL_Template\AVATOOL\Connected Services\UserManagement\configuration91.svcinfo
    <endpoint name="WebServicesSoap" contract="UserManagement.WebServicesSoap" bindingType="basicHttpBinding" 
	address="http://$YOURAVATARSERVER:8972/csp/avpm/WEBSVC.UserManagement.cls" bindingConfiguration="WebServicesSoap">
          <serializedValue>http://$YOURAVATARSERVER:8972/csp/avpm/WEBSVC.UserManagement.cls</serializedValue>
C:\Z_AVATOOL_Template\AVATOOL\Connected Services\UserManagement\Reference.cs
C:\Z_AVATOOL_Template\AVATOOL\Connected Services\UserManagement\Reference.svcmap
    <MetadataSource Address="http://$YOURAVATARSERVER:8972/csp/avpm/WEBSVC.UserManagement.CLS?WSDL=1" Protocol="http" SourceId="1" />
    <MetadataFile FileName="WEBSVC.wsdl" MetadataType="Wsdl" ID="337193fe-25fd-4857-93cd-27b97a9bd861" SourceId="1" 
	SourceUrl="http://$YOURAVATARSERVER:8972/csp/avpm/WEBSVC.UserManagement.CLS?WSDL=1" />
C:\Z_AVATOOL_Template\AVATOOL\Connected Services\UserManagement\WEBSVC.wsdl
      <soap:address location="http://$YOURAVATARSERVER:8972/csp/avpm/WEBSVC.UserManagement.cls" />

After making the changes above, run the following power shell command to verify you replaced ALL references to 
$YOURAVATARSERVER with your avatar server instance name.
PS C:\Z_AVATOOL_Template> gci -Recurse *.*|ForEach-Object{
>> echo $_.Fullname
>> Get-Content $_.Fullname |Select-String "YOURAVATARSERVER"
>> }

The files to change in the AVATROOL\connected services\UserManagement folder are: References.cs, Configuration.svcinfo, Configuration91.svcinfo, Reference.svcmap, WEBSRC.wsdl, 
note it's much easier (and better) to simply delete the folder and it's contents and add the service reference for your avatar server from visual studio, from the solution explorer, 
select the csproj, right click, Add, Service Reference.  Also don't forget to update AVATOOL\app.config (replace $YOURAVATARSERVER with your avatar server instance, live, sbox, or uat 
for testing).  I used MSVS2019.

Good luck, ..., email with any questions.
Chris Kennedy
ckennedy@aspirehealthalliance.org
cgkboston@gmail.com
Aspire Health Alliance
August 12, 2020, update: August 17, 2020.

