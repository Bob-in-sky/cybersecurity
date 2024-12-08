# Windows Domains

Picture the administrator of a small business network with only five computers and employees. In such a tiny network, the admin will be able to configure each computer separately without a problem. The admin well manually log into each computer, create users for whoever will use them, and make specific configurations for each employee's accounts. If a user's computer stops working, the admin will probably go to their place and fix it on-site.

While this sounds plausible, businesses are usually way larger, and tend to suddenly grow to massive scales with many employees across multiple offices. The admin won't be able to manage each computer as a separate entity, manually configure policies for each of the users across the network and provide on-site support for everyone.

To overcome these limitations, the Windows Domain resource can be used. Simply put, a Windows Domain is a group of users and computers under the administration of a given business. The main idea behind a domain is to centralize the administration of common components of a Windows computer network in a single repository called <span style="color: #169179;">Active Directory</span> (AD). The server that runs the Active Directory services is knows as a <span style="color: #169179;">Domain Controller</span> (DA).

<img src="../../_resources/bebe5dfec0208bf563d01fa2dd1fb7a7.png" alt="bebe5dfec0208bf563d01fa2dd1fb7a7.png" width="495" height="445" style="display: block; margin: 0 auto;" class="jop-noMdConv">

The main advantages of having a configured Windows Domain are:

- - **Centralized Identity Management:** All users across the network can be configured from Active Directory with minimum effort.
    - **Managing Security Policies:** Security policies can be configured directly from Active Directory and apply them to users and computers across the network as needed.

&nbsp;

## Real-World Example

If this sounds a bit confusing, chances are that you have already interacted with a Windows domain at some point in your school, university or work.

In school/university networks, you will often be provided with a username and password that you can use on any of the computers available on campus. Your credentials are valid for all machines because whenever you input them on a machine, it will forward the authentication process back to the Active Directory, where your credentials will be checked. Thanks to Active Directory, your credentials don't need to exist in each machine and are available throughout the network.

Active Directory is also the component that allows your school/university to restrict you from accessing the control panel on your school/university machines. Policies will usually be deployed throughout the network so that you don't have administrative privileges over those computers.