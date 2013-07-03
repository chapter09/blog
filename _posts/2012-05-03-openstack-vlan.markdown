---
author: chapter09
comments: true
date: 2012-05-03 12:18:25+00:00
layout: post
slug: openstack-vlan
title: OpenStack VLAN
wordpress_id: 667
categories:
- Tech
tags:
- network
- openstack
---

> VLAN网络模式

OpenStack Compute的默认模式。在这个模式里面，Compute为每个项目创建了VLAN和网桥。为了实现多台机器的安装，VLAN网络模式需要一个支持VLAN标签(IEEE 802.1Q)的路由器。每个项目获得一些只能从VLAN内部访问的私有IP地址。为了实现用户获得项目的实例，需要创建一个特殊的VPN实例（代码名为cloudpipe）。Compute为用户生成了证明书和key，使得用户可以访问VPN，同时Compute自动启动VPN。它为每个项目的所有实例提供一个私有网络段，这个私有网络段都是可以通过因特网的VPN访问的。在这个模式里面，每个项目获得它自己的VLAN，Linux网桥还有子网。被网络管理员所指定的子网都会在需要的时候动态地分配给一个项目。DHCP服务器为所有的VLAN所启动，从被分配到项目的子网中获取IP地址并传输到虚拟机实例。所有属于某个项目的实例都会连接到同一个VLAN。OpenStack Compute在必要的时候会创建Linux网桥和VLAN。



<!-- more -->



> Users and Projects (Tenants)

The OpenStack Compute system is designed to be used by many different cloud computing consumers or customers, basically tenants on a shared system, using role-based access assignments. With the use of the Identity Service, the issuing of a token also issues the roles assigned to the user, and the Identity Service calls projects tenants. Roles control the actions that a user is allowed to perform. For example, a user cannot allocate a public IP without the netadmin or admin role when the system is set up according to those rules. There are both global roles and per-project (tenant) role assignments. A user's access to particular images is limited by project, but the access key and secret key are assigned per user. Key pairs granting access to an instance are enabled per user, but quotas to control resource consumption across available hardware resources are per project.

With the "--use_deprecated_auth" flag in place, OpenStack Compute uses a rights management system that employs a Role-Based Access Control (RBAC) model and supports the following five roles:

Cloud Administrator (admin): Global role. Users of this class enjoy complete system access.

IT Security (itsec): Global role. This role is limited to IT security personnel. It permits role holders to quarantine instances on any project.

Project Manager (projectmanager): Project role. The default for project owners, this role affords users the ability to add other users to a project, interact with project images, and launch and terminate instances.

Network Administrator (netadmin): Project role. Users with this role are permitted to allocate and assign publicly accessible IP addresses as well as create and modify firewall rules.

Developer (developer): Project role. This is a general purpose role that is assigned to users by default.

While the original EC2 API supports users, OpenStack Compute adds the concept of projects, or tenants if your deployment uses the Identity Service (Keystone). Projects and tenants are isolated resource containers forming the principal organizational structure within Nova. They consist of a separate VLAN, volumes, instances, images, keys, and users. A user can specify which project or tenant he or she wishes to be known as by appending :project_id to his or her access key. If no project or tenant is specified in the API request, Compute attempts to use a project with the same id as the user.

For projects (tenants), quota controls are available to limit the:

Number of volumes which may be created

Total size of all volumes within a project as measured in GB

Number of instances which may be launched

Number of processor cores which may be allocated

Publicly accessible IP addresses




