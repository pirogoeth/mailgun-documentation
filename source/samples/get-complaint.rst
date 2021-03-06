
.. code-block:: bash

    curl -s --user 'api:YOUR_API_KEY' -G \
	https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/complaints/baz@example.com

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

     public static ClientResponse GetComplaint() {

         Client client = ClientBuilder.newClient();
         client.register(HttpAuthenticationFeature.basic(
             "api",
             "YOUR_API_KEY"
         ));

         WebTarget mgRoot = client.target("https://api.mailgun.net/v3");

         return mgRoot
             .path("/{domain}/complaints/{address}")
             .resolveTemplate("domain", "YOUR_DOMAIN_NAME")
             .resolveTemplate("address", "bob@example.com")
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
  $domain = 'YOUR_DOMAIN_NAME';
  $complaint = 'user@example.com';

  # Issue the call to the client.
  $result = $mgClient->get("$domain/complaints/$complaint");

.. code-block:: py

 def get_complaint():
     return requests.get(
         "https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/complaints/baz@example.com",
         auth=("api", "YOUR_API_KEY"))

.. code-block:: rb

 def get_complaint
   RestClient.get("https://api:YOUR_API_KEY"\
                  "@api.mailgun.net/v3/YOUR_DOMAIN_NAME/complaints/"\
                  "baz@example.com"){|response, request, result| response }
 end

.. code-block:: csharp

 using System;
 using System.IO;
 using RestSharp;
 using RestSharp.Authenticators;
 
 public class GetComplaintChunk
 {
 
     public static void Main (string[] args)
     {
         Console.WriteLine (GetComplaint ().Content.ToString ());
     }
 
     public static IRestResponse GetComplaint ()
     {
         RestClient client = new RestClient ();
         client.BaseUrl = new Uri ("https://api.mailgun.net/v3");
         client.Authenticator =
             new HttpBasicAuthenticator ("api",
                                         "YOUR_API_KEY");
         RestRequest request = new RestRequest ();
         request.AddParameter ("domain", "YOUR_DOMAIN_NAME", ParameterType.UrlSegment);
         request.Resource = "{domain}/complaints/baz@example.com";
         return client.Execute (request);
     }
 
 }

.. code-block:: go

 func GetComplaints(domain, apiKey string) (mailgun.Complaint, error) {
   mg := mailgun.NewMailgun(domain, apiKey, "")
   return mg.GetSingleComplaint("baz@example.com")
 }
