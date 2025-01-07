<h1> Rapid deployment of the YuniKorn on Alibaba Cloud compute nest </h1>

<blockquote>
    <p><strong> Disclaimer </strong>: This service is provided by a third party. We try our best to ensure its security,
        accuracy and reliability, but we cannot guarantee that it is completely free from failure, interruption, error
        or attack. Therefore, the company hereby declares that it makes no representations, warranties or commitments
        regarding the content, accuracy, completeness, reliability, suitability and timeliness of the Service and is not
        liable for any direct or indirect loss or damage arising from your use of the Service; for third-party websites,
        applications, products and services that you access through the Service, do not assume any responsibility for
        its content, accuracy, completeness, reliability, applicability and timeliness, and you shall bear the risks and
        responsibilities of the consequences of use; for any loss or damage arising from your use of this service,
        including but not limited to direct loss, indirect loss, loss of profits, loss of goodwill, loss of data or
        other economic losses, even if we have been advised in advance of the possibility of such loss or damage; we
        reserve the right to amend this statement from time to time, so please check this statement regularly before
        using the Service. If you have any questions or concerns about this Statement or the Service, please contact us.
    </p>
</blockquote>

<h2> Overview </h2>

<p>YuniKorn is a lightweight, universal resource scheduler designed for container orchestration systems. It was created to efficiently achieve fine-grained resource sharing for various workloads in large-scale, multi-tenant environments, while also dynamically creating cloud-native environments. YuniKorn provides a unified cross-platform scheduling experience for hybrid workloads, including stateless batch workloads and stateful services, and supports, but is not limited to, YARN and Kubernetes.</p>
<p>
    Address of this project: <a href='https://yunikorn.apache.org/'>https://yunikorn.apache.org/</a></p>

<h2> Prerequisites </h2>

<p><font style="color:rgb(51, 51, 51);"> To deploy a YuniKorn community edition service instance, you need to
    access and create some Alibaba Cloud resources. Therefore, your account must contain permissions for the following
    resources. </font><font style="color:rgb(51, 51, 51);"> </font><strong><font style="color:rgb(51, 51, 51);">
    Description </font></strong><font style="color:rgb(51, 51>: 51);">: this permission is required only when your account is a RAM account. </font></p>

<table>
<thead>
<tr>
<th><font style = " color:rgb(51, 51, 51);"> Permission policy name </font></th>
    <th><font style="color:rgb(51, 51, 51);"> Remarks </font></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td><font style="color:rgb(51, 51, 51);">AliyunECSFullAccess</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Permissions to manage ECS </font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);">AliyunCSFullAccess</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Permissions to manage CS </font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);">AliyunVPCFullAccess</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Permissions to manage a VPC </font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);">AliyunROSFullAccess</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Manage permissions for the Resource Orchestration Service
            (ROS) </font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);">AliyunComputeNestUserFullAccess</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Manage user-side permissions for the compute nest service
            (ComputeNest) </font></td>
    </tr>
    </tbody>
    </table>

<h2> Billing instructions </h2>

<p><font style="color:rgb(51, 51, 51);"> The cost of YuniKorn community edition deployment in computing nest
    mainly involves:</font></p>

<ul>
    <li><font style="color:rgb(51, 51, 51);"> selected vCPU and memory specifications </font></li>
    <li><font style="color:rgb(51, 51, 51);"> System disk type and capacity </font></li>
    <li><font style="color:rgb(51, 51, 51);"> Internet bandwidth </font></li>
</ul>

<p> This service requires ECS instance can access YuniKorn server from public network. </p>

<h2> Deployment Architecture </h2>

<p> This service is deployed on a single ECS instance. The architecture is as follows:</p>

<p><img src="./1.png" alt=""/></p>

<h2> Parameter description </h2>

<table>
    <thead>
    <tr>
        <th><font style="color:rgb(51, 51, 51);"> parameter group </font></th>
        <th><font style="color:rgb(51, 51, 51);"> parameter items </font></th>
        <th><font style="color:rgb(51, 51, 51);"> Description </font></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td><font style="color:rgb(51, 51, 51);"> Service instance </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Service instance name </font></td>
        <td><font style="color:rgb(51, 51, 51);"> No more than 64 characters in length, must start with an English
            letter, and can contain numbers, English letters, dashes (-), and underscores (_)</font></td>
    </tr>
    <tr>
        <td></td>
        <td><font style="color:rgb(51, 51, 51);"> Region </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Region where the service instance is deployed </font></td>
    </tr>
    <tr>
        <td></td>
        <td><font style="color:rgb(51, 51, 51);"> Payment type </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Resource billing type: Pay-As-You-Go and Subscription </font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);"> resource configuration </font></td>
        <td><font style="color:rgb(51, 51, 51);"> instance type </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Instance specifications available in the Availability Zone </font>
        </td>
    </tr>
    <tr>
        <td></td>
        <td><font style="color:rgb(51, 51, 51);"> Instance password </font></td>
        <td><font style="color:rgb(51, 51, 51);"> is 8-30 in length and must contain three items (uppercase letters,
            lowercase letters, numbers, ()'~! @#$%^& *-+ ={}[]:;,.? Special symbols in)</font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);"> Availability Zone Configuration </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Availability Zone </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Zone where the ECS instance is located </font></td>
    </tr>
    <tr>
        <td></td>
        <td><font style="color:rgb(51, 51, 51);">VPC ID</font></td>
        <td><font style="color:rgb(51, 51, 51);"> VPC where the resource is located </font></td>
    </tr>
    <tr>
        <td></td>
        <td><font style="color:rgb(51, 51, 51);"> Switch ID</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Switch where the resource is located </font></td>
    </tr>
    </tbody>
</table>

<h2> Deployment process </h2>

<ol>
    <li> Visit the compute nest YuniKorn<a
            href="https://computenest.console.aliyun.com/service/instance/create/default?type=user&ServiceName=YuniKorn%E7%A4%BE%E5%8C%BA%E7%89%88">
        deployment link </a> and fill in the deployment parameters as prompted
    </li>
    <li> Select payment type
        <img src="./9.jpg" alt=""/></li>
    <li> Enter instance parameters and fill in the zone, and click Next: Confirm Order
        <img src="./10.jpg" alt=""/></li>
    <li> Confirm all parameters and estimate price, click Create Now and wait for the service instance deployment to complete</li>
    <li> After the service instance is deployed, click the instance ID to go to the details page. <img src="./11.jpg" alt=""/><img src="./12.jpg" alt=""/>
    </li>
    <li> Access the YuniKorn dashboard </li> <img src="./7.jpg" alt=""/><img src="./8.jpg" alt=""/>
</ol>