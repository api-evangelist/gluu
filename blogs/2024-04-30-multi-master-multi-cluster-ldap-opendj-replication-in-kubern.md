---
title: "Multi Master Multi-Cluster LDAP (OpenDJ) replication in Kubernetes? A controversial view"
url: "https://gluu.org/opendj-is-a-lightweight-directory-access-protocol-ldap-compliant-distributed-directory-written-in-java-many-organizations-use-it-as-a-persistence-mechanism-for-their-iam-systems/"
date: "Tue, 30 Apr 2024 08:38:24 +0000"
author: "Via Bialkowski"
feed_url: "https://gluu.org/feed/"
---
<div class="elementor elementor-28687">
				<div class="elementor-element elementor-element-e2b4420 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
		<div class="elementor-element elementor-element-8e478b9 e-con-full e-flex e-con e-child">
				<div class="elementor-element elementor-element-4f227ac elementor-align-center elementor-widget elementor-widget-post-info">
				<div class="elementor-widget-container">
							<ul class="elementor-inline-items elementor-icon-list-items elementor-post-info">
								<li class="elementor-icon-list-item elementor-repeater-item-e8b2426 elementor-inline-item">
										<span class="elementor-icon-list-icon">
								<i class="far fa-tags"></i>							</span>
									<span class="elementor-icon-list-text elementor-post-info__item elementor-post-info__item--type-custom">
										Mohammad Abudayyeh - Director of DevOps @ Gluu					</span>
								</li>
				</ul>
						</div>
				</div>
				<div class="elementor-element elementor-element-c13eefa elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<h3>Multi-Master Multi-Cluster LDAP (OpenDJ) replication in Kubernetes? A controversial view</h3>
OpenDJ is a Lightweight Directory Access Protocol (LDAP) compliant distributed directory written in Java. Many organizations use it as a persistence mechanism for their IAM systems. But should it be used in a multi cluster or multi regional <a href="https://kubernetes.io/" rel="noopener" target="_blank">Kubernetes</a> setup by organizations? That is exactly what we want to discuss and dive into.
<h3>Beware the initial YES</h3>
Gluu Federation had great results with <a href="https://github.com/GluuFederation/gluu-opendj4" rel="noopener" target="_blank">OpenDJ </a> working in a multi-AZ single Kubernetes cluster. As a result, Gluu Federation decided that the engineering team should take on the task of supporting <a href="https://gluu.org/docs/gluu-server/4.3/tutorials/cn-ldap-multi-cluster" rel="noopener" target="_blank">multi-regional OpenDJ in Kubernetes setups</a>. We realized that <a href="https://github.com/GluuFederation/gluu-opendj4" rel="noopener" target="_blank">OpenDJ </a>needed a lot of work. The containerization team looked at one of the most probable issues that would arise, which are replication issues. Looking at the code, <a href="https://github.com/GluuFederation/gluu-opendj4" rel="noopener" target="_blank">OpenDJ </a>lacked a lot of the networking smarts that would ease the addition and removal of regional pods. The solution was to match <a href="https://www.serf.io/" rel="noopener" target="_blank">Serf</a> with our <a href="https://github.com/GluuFederation/docker-opendj/tree/4.3" rel="noopener" target="_blank">OpenDJ container image</a> and use it as the intelligence layer for pod membership, OpenDJ pod failure detection, and orchestration. That layer would communicate effectively with the replication process <a href="https://github.com/GluuFederation/gluu-opendj4" rel="noopener" target="_blank">OpenDJ </a>holds through NodePorts or static addresses assigned per pod.								</div>
				</div>
				<div class="elementor-element elementor-element-d6fa6eb elementor-widget elementor-widget-image">
				<div class="elementor-widget-container">
															<img alt="" src="https://gluu.org/wp-content/uploads/2021/11/multimaster-multi-cluster-1.png" title="" />															</div>
				</div>
				<div class="elementor-element elementor-element-7afa62d elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><em>OpenDJ communication</em></p>								</div>
				</div>
				<div class="elementor-element elementor-element-64491f9 elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p>Gluu Federation <a href="https://github.com/GluuFederation/cloud-native-edition/tree/4.3/pygluu/kubernetes/templates/helm/gluu" rel="noopener" target="_blank">helm charts</a> holding the <a href="https://github.com/GluuFederation/cloud-native-edition/tree/4.3/pygluu/kubernetes/templates/helm/gluu/charts/opendj/templates" rel="noopener" target="_blank">OpenDJ sub chart</a> also took care of creating the needed Kubernetes resources to support a multi-regional setup. After several rounds of testing</p><p>Gluu rolled it out in Alpha stage to some customers. Although the solution worked we started noticing several issues:</p><p><strong>Replication!</strong> Depending on how fast replications occurs and the user login rate, users may face login issues. Yes, this may occur in a single regional Kubernetes cluster but it is much less observable. To solve this, the user needed to be tied with a session and later reconcile the replication. Unless the user travels to another region before replication fully re balances that user won’t notice.</p><p><strong>Missing Changes (M.C)!</strong> This was the main reason</p><p>Gluu Federation decided to withdraw the alpha support and remove the layers to support multi regional multi master OpenDJ setups in Kubernetes. Several missing changes started appearing in the long run that would not sync in. This would require manual intervention to force the sync and activate the replication correctly. Something many don’t want to be handling in a production environment. The below is one said replication status:</p>								</div>
				</div>
				<div class="elementor-element elementor-element-570ebe9 elementor-widget elementor-widget-code-highlight">
				<div class="elementor-widget-container">
							<div class="prismjs-default copy-to-clipboard ">
			<pre class="highlight-height language-javascript line-numbers">
				<code class="language-javascript" readonly="true">
					Suffix DN : Server                                                       : Entries : Replication enabled : DS ID : RS ID : RS Port (1) : M.C. (2) : A.O.M.C. (3) : Security (4)
----------:--------------------------------------------------------------:---------:---------------------:-------:-------:-------------:----------:--------------:-------------
o=gluu    : gluu-opendj-east-regional-0-regional.gluu.org:30410 : 68816   : true                : 30177 : 21360 : 30910       : 0        :              : true
o=gluu    : gluu-opendj-east-regional-1-regional.gluu.org:30440 : 66444   : true                : 13115 : 26513 : 30940       : 0        :              : true
o=gluu    : gluu-opendj-west-regional-0-regional.gluu.org:30441 : 67166   : true                : 16317 : 17418 : 30941       : 0        :              : true
o=metric  : gluu-opendj-east-regional-0-regional.gluu.org:30410 : 30441   : true                : 30271 : 21360 : 30910       : 0        :              : true
o=metric  : gluu-opendj-east-regional-1-regional.gluu.org:30440 : 24966   : true                : 30969 : 26513 : 30940       : 0        :              : true
o=metric  : gluu-opendj-west-regional-0-regional.gluu.org:30441 : 30437   : true                : 1052  : 17418 : 30941       : 0        :              : true
o=site    : gluu-opendj-east-regional-0-regional.gluu.org:30410 : 133579  : true                : 1683  : 21360 : 30910       : 0        :              : true
o=site    : gluu-opendj-east-regional-1-regional.gluu.org:30440 : 133425  : true                : 15095 : 26513 : 30940       : 0        :              : true
o=site    : gluu-opendj-west-regional-0-regional.gluu.org:30441 : 133390  : true                : 26248 : 17418 : 30941       : 0        :              : true
				</code>
			</pre>
		</div>
						</div>
				</div>
				<div class="elementor-element elementor-element-80108b6 elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><strong>Maintenance!</strong> No matter how good of a solution it was it required constant care unlike modern central managed NOSQL or even RDBMS solutions.</p><p><strong>Recovery! </strong>Not as easy as thought , but recovery can go wrong easily since the operations are occurring on an ephemeral environment. Sometimes this requires shutting down a regions persistence and preform the recovery on one side then bringing the subsequent region afterwards. Not so cloud friendly right?!</p><p><strong>Cost! </strong>Taking a look at the cost organizations spend on just holding a setup as mentioned up is actually higher then simply using a cloud managed solution. Of course given that the cloud managed solution is viable as some organizations require all services to live on their data centers.</p><p><strong>Scale! </strong>No matter how smart the solution was, scaling up and down on higher rates of authentications was an issue. Going back to the first issue mentioned, if a high surge occurred and replication was behind a bit some authentications will fail. What would happen if auto scaling was enabled and a high surge occurred? Obviously, the number of pods would increase and hence the number of available members that need to join OpenDJ increases.</p><p>While pods become ready to accept traffic several users will be directed to different pods depending on the routing strategy creating a bigger gap for replication differences to arise. If the surge discontinues before replication fully balances two scenarios may occur. The first is the normal wanted behavior where the pod would check if replication with its peers is perfect and there are no missing changes in which the pod de-registers itself from the cluster membership and then gracefully shuts down.</p><p>The other behavior is where a missing change arises and the pod can’t scale down hence holding up the resources until an engineer can walk in, look at the issue, fix it and possibly forcefully scale down. I’m not saying a solution can’t be made here but the price for making it available doesn’t make sense with more viable solutions such as RDBMS and NOSQL solutions.</p><p><strong>Performance! </strong>In general, we noticed that any organization requiring larger then 200–300 authentications per second should avoid using OpenDJ as the persistence layer. You do not want to be handling replications issues with 200 authentications per second coming in.</p><h3>Conclusion</h3><p>I personally still love OpenDJ and the LDAP protocol in general but is it a cloud friendly product? Not really. I would still recommend it for single multi AZ Kubernetes cluster for small — medium organizations that are trying to save money and have a light load. Focusing on what matters for your organization is very important. If you can implement a central managed solution, and the cost for you is relatively low, you should go with a managed solution that can scale and replicate with minimal effort across regions.</p>								</div>
				</div>
				</div>
					</div>
				</div>
				</div><p>The post <a href="https://gluu.org/opendj-is-a-lightweight-directory-access-protocol-ldap-compliant-distributed-directory-written-in-java-many-organizations-use-it-as-a-persistence-mechanism-for-their-iam-systems/">Multi Master Multi-Cluster LDAP (OpenDJ) replication in Kubernetes? A controversial view</a> first appeared on <a href="https://gluu.org">Gluu</a>.</p>
