# Questions & Answers

* **Question:** in CF, there is a command "cf stacks". What is the semantics of a "stack" in the CloudFoundry world? -The documentation on the Cloud Foundry Wiki (http://docs.pivotal.io/pivotalcf/concepts/stacks.html) is not really helpful...

* **Question:** should there be only one ELK stack per CloudFoundry instance? If the whole SAP only has one single cloud foundry instance for all projects, should there then also only be one single ELK stack, or rather one stack per project/product/org/...?

  * **Answer:** There are two options:
    * Provide seperate ELK stack per customer/account
    * Provide only one single instance, and have an authorization concept so that log data from one account is not visible to other accounts.

    The latter choice is the preferred, however HCP will in the first delivery provide option 1 only, because the authorization concept is not yet finalized / applicable.
    
* **Question:** if there are multiple CF instances (e.g. one for development and one for production), do we still use only one single ELK stack for all of them?

* **Question:** (C. Otto) Is it possible to scale the logging setup including the ELK stack? As far as I know, one needs several Doppler instances (part of Cloud Foundry), in addition to several Elastic Search / Logstash instances.
