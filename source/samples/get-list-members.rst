
.. code-block:: bash

    curl -s --user 'api:YOUR_API_KEY' -G \
	https://api.mailgun.net/v3/lists/LIST@YOUR_DOMAIN_NAME/members/pages

.. code-block:: java

 import javax.ws.rs.client.Client;
 import javax.ws.rs.client.ClientBuilder;
 import javax.ws.rs.client.Entity;
 import javax.ws.rs.client.WebTarget;

 import javax.ws.rs.core.Form;
 import javax.ws.rs.core.MediaType;

 import org.glassfish.jersey.client.authentication.HttpAuthenticationFeature;

 public class MGSample {

     // ...

     public static ClientResponse GetListMembers() {

         Client client = ClientBuilder.newClient();
         client.register(HttpAuthenticationFeature.basic(
             "api",
             "YOUR_API_KEY"
         ));

         WebTarget mgRoot = client.target("https://api.mailgun.net/v3");

         return mgRoot
             .path("/lists/{list_name}@{domain}/members/pages")
             .resolveTemplate("domain", "YOUR_DOMAIN_NAME")
             .resolveTemplate("list_name", "YOUR_MAILING_LIST_NAME")
             .request()
             .buildGet()
             .invoke(ClientResponse.class);
     }
 }

.. code-block:: php

  # Include the Autoloader (see "Libraries" for install instructions)
  require 'vendor/autoload.php';
  use Mailgun\Mailgun;

  # Instantiate the client.
  $mgClient = new Mailgun('YOUR_API_KEY');
  $listAddress = 'LIST@YOUR_DOMAIN_NAME';

  # Issue the call to the client.
  $result = $mgClient->get("lists/$listAddress/members/pages", array(
      'subscribed' => 'yes',
      'limit'      =>  5
  ));

.. code-block:: py

 def list_members():
     return requests.get(
         "https://api.mailgun.net/v3/lists/LIST@YOUR_DOMAIN_NAME/members/pages",
         auth=('api', 'YOUR_API_KEY'))

.. code-block:: rb

 def list_members
   RestClient.get("https://api:YOUR_API_KEY" \
                  "@api.mailgun.net/v3/lists/LIST@YOUR_DOMAIN_NAME/members/pages")
 end

.. code-block:: csharp

 using System;
 using System.IO;
 using RestSharp;
 using RestSharp.Authenticators;
 
 public class GetListMembersChunk
 {
 
     public static void Main (string[] args)
     {
         Console.WriteLine (GetListMembers ().Content.ToString ());
     }
 
     public static IRestResponse GetListMembers ()
     {
         RestClient client = new RestClient ();
         client.BaseUrl = new Uri ("https://api.mailgun.net/v3");
         client.Authenticator =
             new HttpBasicAuthenticator ("api",
                                         "YOUR_API_KEY");
         RestRequest request = new RestRequest ();
         request.Resource = "lists/{list}/members/pages";
         request.AddParameter ("list", "LIST@YOUR_DOMAIN_NAME",
                               ParameterType.UrlSegment);
         return client.Execute (request);
     }
 
 }

.. code-block:: go

 func GetMembers(domain, apiKey string) (int, []mailgun.Member, error) {
   mg := mailgun.NewMailgun(domain, apiKey, "")
   return mg.GetMembers(-1, -1, mailgun.All, "LIST@YOUR_DOMAIN_NAME")
 }
