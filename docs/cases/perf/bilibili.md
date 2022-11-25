# Bilibili.com Case
Bilibili, China’s biggest streaming and video sharing site. The incident happened with Bilibili's internal gateway systems which could not handle more requests and were running at 100% on CPU.

**Entity:** Bilibili.com

**Date:** July 2021, August 2022

**Error:** Software. Runtime data type conversion error

**Component:** OpenResty

**Programming Language:** Lua

**Operating System:** Unknown

**Type:** Poor performance, service outage

**Synopsis:** "The Lua flame graph eventually located the culprit — a string-type zero value for a server weight (i.e., “0”) was inserted into the business logic’s configuration metadata, while the Lua API of OpenResty’s lua-resty-balancer library expected a numerical weight value. It thus led to infinite recursion and infinite loops."

## Resolving Bilibili’s major site incident with OpenResty XRay

**Author:** Yichun Zhang
Check out how OpenResty XRay helps organizations troubleshoot issues and optimize the performance of their applications.


### LEARN MORE
OpenResty Inc. used the proprietary product OpenResty XRay with its enhanced dynamic tracing technology to resolve a significant website crash of Bilibili.com, one of the busiest streaming websites in Asia. After hours of troubleshooting, the engineering team at Bilibili.com reached out to the OpenResty team for assistance. With OpenResty XRay, the OpenResty team quickly pinpointed the root cause and resolved the major incident in production.

### OpenResty XRay Robots

A recent article entitled 2021.07.13 This is how we crashed by Bilibili.com summarized a significant site crash that occurred last year. This article sparked a lot of questions, discussions, and interest in the specifics. Therefore, we would like to take this opportunity to explain how OpenResty helped handle this incident and to introduce the tools we used to a larger audience that might be interested in the technical details.

As a startup, we mainly focused on engineering innovation in the past few years while seldom advertising our commercial products. This post is the first in a series that introduces OpenResty XRay to a wider audience so that we can help more companies quickly troubleshoot and unravel mysterious application problems.

Bilibili, China’s biggest streaming site

Bilibili is a Shanghai-based company, also known as B-site. It has positioned itself as the number one anime streaming site; it is also one of the most popular video-sharing platforms in Asia.

### The incident

Background: Bilibili developed its internal gateway system based on the Open Source OpenResty development framework.

Incident: When the incident happened, the OpenResty processes on all the online servers at Bilibili were using 100% of the CPU but could not handle any requests. All servers experienced the same issue, and rebooting or rolling back didn’t help.

CPU All 100%

Bilibili is a commercial customer of OpenResty XRay. The engineering team at Bilibili contacted us for assistance in analyzing and resolving the problem in production after exhausting all of their internal resources.

### Process of resolution

Through the automatic sampling C-level CPU flame-graph in OpenResty XRay, we quickly identified that the Bilibili system’s OpenResty Nginx process was spending almost all of its CPU time executing Lua code.

C-Land CPU Flame Grahp for Bilibili

For privacy and data security reasons, this graph only shows the function frames from OpenResty’s open source software, not including the Lua code and relative information from Bilibili.

We then used OpenResty XRay’s automatic sampling Lua level CPU flame graph to confirm that almost all CPU time is used on one Lua code path, which is in an infinite loop. Our Lua CPU Flame graph can show details on the granularity level of Lua source code lines.

Lua-Land CPU Flame Graph for Bilibili

Again, we only keep function frames from the non-customer-related open source code.

The Lua flame graph mentioned in the original article from Bilibili was obtained by sampling the problematic OpenResty service process on the Bilibili production server using OpenResty XRay.

It only took OpenResty XRay a few seconds to generate the flame graphs, which indicated the exact code paths containing the root causes. The entire troubleshooting process did not require any code modifications or injection to the system process or application. Its 100% non-invasive dynamic tracing proprietary technology makes OpenResty XRay stand out from the rest of the APM products in the market.

The Lua flame graph eventually located the culprit — a string-type zero value for a server weight (i.e., “0”) was inserted into the business logic’s configuration metadata, while the Lua API of OpenResty’s lua-resty-balancer library expected a numerical weight value. It thus led to infinite recursion and infinite loops.

### Hardening after the incident

Bilibili ensures that no string-typed weight values for upstream servers will ever get written to the configuration data in the business logic code in the future.

The latest version of OpenResty XRay provides a new feature that prints the values of all local variables on the Lua call stack, ensuring that issues like this can be located more quickly and directly.

We have also explicitly hardened the open-source OpenResty lua-resty-balancer library against such API misuses, ensuring that any wrong weight values of incorrect data types will always be converted to numeric types.

### OpenResty XRay Services

OpenResty XRay is a non-invasive troubleshooting and analytic software utilizing enhanced proprietary dynamic tracing technology. With OpenResty XRay, companies can automatically perform in-depth analysis and monitoring on software built with open-source programming languages such as OpenResty, Nginx, LuaJIT, PHP, Python, Perl, Go, PostgreSQL, Redis, etc. To provide much greater analytics power to users, we are also adding support for more technology stacks, including Java, Ruby, etc. With OpenResty XRay, users can quickly identify and pinpoint performance, functional, and security issues to ensure the system’s stability in all environments.

OpenResty XRay’s code-level troubleshooting requires zero modifications in users’ applications, no special plug-ins, modules, or compilation options, and no code injection into the customer’s processes. It can also penetrate the customer’s existing unmodified Docker or Kubernetes (K8s) containers and precisely analyze the application inside them.

In addition to Bilibili, OpenResty XRay has also successfully helped Zoom, Microsoft, Qunar.com, and many other companies optimize performance, and pinpoint issues in products; ranging from high CPU spikes to high memory usage to memory leaks, abnormal request latency, high hard disk IO, and more.

OpenResty Console Screenshot

### Closing

The incident was perfectly resolved in the end. We appreciate Bilibili trusting our team and products throughout the process!

If you are interested in learning more about OpenResty XRay and would like to receive a FREE consultation report from our domain experts, please visit the OpenResty XRay product page and request a product trial.

For those interested in dynamic tracing and thclosed-sourcece enhancement we built on top, please check out Ylang: Universal Language for eBPF, Stap+, GDB, and More.

Apply for OpenResty XRay trial NOW and receive a FREE report
About The Author

Yichun Zhang (Github handle: agentzh), is the original creator of the OpenResty® open-source project and the CEO of OpenResty Inc..

Yichun is one of the earliest advocates and leaders of “open-source technology”. He worked at many internationally renowned tech companies, such as Cloudflare, Yahoo!. He is a pioneer of “edge computing”, “dynamic tracing” and “machine coding”, with over 22 years of programming and 16 years of open source experience. Yichun is well-known in the open-source space as the project leader of OpenResty®, adopted by more than 40 million global website domains.

OpenResty Inc., the enterprise software start-up founded by Yichun in 2017, has customers from some of the biggest companies in the world. Its flagship product, OpenResty XRay, is a non-invasive profiling and troubleshooting tool that significantly enhances and utilizes dynamic tracing technology.

As an avid open-source contributor, Yichun has contributed more than a million lines of code to numerous open-source projects, including Linux kernel, Nginx, LuaJIT, GDB, SystemTap, LLVM, Perl, etc. He has also authored more than 60 open-source software libraries

# References 
https://blog.openresty.com/en/bilibili-xray-incident/ 
https://mp.weixin.qq.com/s/nGtC5lBX_Iaj57HIdXq3Qg 