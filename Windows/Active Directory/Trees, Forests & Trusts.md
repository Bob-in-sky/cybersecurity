# Trees, Forests & Trusts

So far, we have discussed how to manage a single domain, the role of a Domain Controller and how it joins computers, servers and users.

<img src="../../_resources/69f2441bbafd4cfe57a101d87f3c5950.png" alt="69f2441bbafd4cfe57a101d87f3c5950.png" width="294" height="176" class="jop-noMdConv" style="display: block; margin: 0 auto;">

As companies grow, so do their networks. Having a single domain for a company is good enough to start, but in time some additional needs might push you into having more than one.

&nbsp;

## Trees

Imagine, for example, that suddenly your company expands to a new country. The new country has different laws and regulations that require you to update your GPOs to comply. In addition, you now have IT people in both countries, and each IT team needs to manage the resources that correspond to each country without interfering with the other team. While you could create a complex OU structure and use delegations to achieve this, having a huge AD structure might be hard to manage and prone to human errors.

Active Directory supports integrating multiple domains so that you can partition your network into units that can be managed independently. If you have two domains that share the same namespace (thm.local for example), those domains can be joined into a <span style="color: #2dc26b;">**Tree**</span>.

If the thm.local domain was split into the subdomains for UK and US branches, you could build a tree with a root domain of thm.local and two subdomains called uk.thm.local and us.thm.local, each with its AD, computers and users:

<img src="../../_resources/abea24b7979676a1dcc0c568054544c8.png" alt="abea24b7979676a1dcc0c568054544c8.png" width="372" height="294" class="jop-noMdConv" style="display: block; margin: 0 auto;">

This partitioned structure gives us better control over who can access what in the domain. The IT people from the UK will have their own DC that manages the UK resources only. For example, a UK user would not be able to manage US users. In that way, the Domain Administrators of each branch will have complete control over their respective DCs, but not other branches's DCs. Policies can also be configured independently for each domain in the tree.

A new security group needs to be introduced when talking about trees and forests. The **Enterprise Admins** group will grant a user administrative privileges over all of an enterprise's domains. Each domain would still have its Domain Admins with administrator privileges over their single domains and the Enterprise Admins who can control everything in the enterprise.

&nbsp;

## Forests

The domains you manage can also be configured in different namespaces. Suppose you company continues growing and eventually acquires another company called <span style="color: #2dc26b;">MHT Inc</span>. When both companies merge, you will probably have different domain trees for each company, each managed by its own IT department. The union of several trees with different namespaces into the same network is known as a **<span style="color: #2dc26b;">Forest</span>**.

<img src="../../_resources/03448c2faf976db890118d835000bab7.png" alt="03448c2faf976db890118d835000bab7.png" width="553" height="223" class="jop-noMdConv" style="display: block; margin: 0 auto;">

&nbsp;

## Trust Relationships

Having multiple domains organised in trees and forests allows you to have a nice compartmentalized network in terms of management and resources. Bur at a certain point, a user at THM UK might need to access a shared folder in one of the MHT ASIA servers. For this to happen, domains arranged in trees and forests are joined together by **trust relationships**.

In simple terms, having a trust relationship between domains allows you to authorize a user from domain <span style="color: #2dc26b;">THM UK</span> to access resources from domain <span style="color: #2dc26b;">MHT ASIA</span>.

The simplest trust relationship that can be established is a one-way trust relationship. In a one-way trust, if <span style="color: #2dc26b;">Domain AAA</span> trusts <span style="color: #2dc26b;">Domain BBB</span>, this means that a user on BBB can be authorized to access resources from AAA:

<img src="../../_resources/af95eb1a4b6c672491d8989f79c00200.png" alt="af95eb1a4b6c672491d8989f79c00200.png" width="522" height="209" class="jop-noMdConv" style="display: block; margin: 0 auto;">

The direction of the one-way trust relationship is contrary to that of the access direction.

**Two-way trust relationships** can also be made to allow both domains to mutually authorize users from each other. By default, joining several domains under a tree or a forest will form a two-way trust relationship.

It is important to not that having a trust relationship between domains doesn't automatically grant access to all resources on other domains. Once a trust relationship is established, you have the chance to authorize users across different domains, but it's up to you what is actually authorized or not.