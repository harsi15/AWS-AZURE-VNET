<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Site-to-Site VPN Connection between Azure and AWS</title>
</head>
<body>
    <h1>Site-to-Site VPN Connection between Azure and AWS</h1>
    <h2>Table of Contents</h2>
    <ul>
        <li><a href="#introduction">Introduction</a></li>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#steps">Steps</a></li>
        <li><a href="#verification">Verification</a></li>
    </ul>
    <h2 id="introduction">Introduction</h2>
    <p>This guide provides instructions for setting up a Site-to-Site VPN connection between AWS and Azure. By following these steps, you will establish a secure connection between your AWS and Azure environments, allowing secure data transfer between resources in each cloud.</p>
    <h2 id="prerequisites">Prerequisites</h2>
    <ul>
        <li>Access to an AWS account with permissions to create VPCs, subnets, and VPN gateways.</li>
        <li>Access to an Azure account with permissions to create Virtual Network Gateways and connections.</li>
        <li>Azure public IP address for the Customer Gateway configuration in AWS.</li>
    </ul>
    <h2 id="steps">Steps</h2>
    <h3>1. Create a VPC in AWS</h3>
    <p>Create a new VPC in AWS with a CIDR block that differs from any existing Azure VPC CIDR to avoid overlap.</p>
    <h3>2. Create a Subnet in the VPC</h3>
    <p>Within the VPC, create a subnet. Ensure that the subnet CIDR falls within the range of the VPC CIDR.</p>
    <h3>3. Create a Customer Gateway</h3>
    <p>Set up a Customer Gateway in AWS by providing the Azure public IP address.</p>
    <h3>4. Create a Virtual Private Gateway</h3>
    <p>Create a Virtual Private Gateway (VPG) in AWS and attach it to the newly created VPC.</p>
    <h3>5. Create a Site-to-Site VPN Connection</h3>
    <ol>
        <li>Select the Virtual Private Gateway created in step 4 as the target gateway type.</li>
        <li>Choose the Customer Gateway created in step 3.</li>
        <li>For routing options, select <strong>Static</strong>, as BGP was not used in Azure.</li>
        <li>Enter the static IP address of the Azure subnet.</li>
        <li>Wait for the status to change from <code>pending</code> to <code>available</code>. This may take 30-40 minutes.</li>
    </ol>
    <h3>6. Download the VPN Configuration File</h3>
    <p>Once the VPN connection is available, download the configuration file with the following selections:</p>
    <ul>
        <li><strong>Vendor:</strong> Generic</li>
        <li><strong>Platform:</strong> Generic</li>
        <li><strong>Software:</strong> Vendor Agnostic</li>
    </ul>
    <h3>7. Configure Local Network Gateway in Azure</h3>
    <p>In Azure, create a Local Network Gateway with the following configuration:</p>
    <ul>
        <li>Copy the <strong>Outside IP Address</strong> from the AWS configuration file to set as the Virtual Private Gateway IP.</li>
        <li>Add the AWS VPC CIDR block to the <strong>Address Spaces</strong>.</li>
    </ul>
    <h3>8. Create Connection in Azure Virtual Network Gateway</h3>
    <ol>
        <li>Go to the Azure Virtual Network Gateway and add a connection.</li>
        <li>Provide the shared key from the AWS configuration file.</li>
        <li>Leave all other settings as default and create the connection.</li>
        <li>Wait for the connection to be established.</li>
    </ol>
    <h3>9. Add Routing Rules</h3>
    <p>Add two routing rules to enable proper traffic flow between AWS and Azure environments.</p>
    <h2 id="verification">Verification</h2>
    <p>Once the setup is complete, verify the VPN connection status in both AWS and Azure to ensure it’s active and traffic can pass between the environments.</p>
    <h3>Troubleshooting</h3>
    <ul>
        <li>Check network security groups and firewall settings on both AWS and Azure to allow the necessary traffic.</li>
        <li>Verify routing tables in AWS and Azure to ensure proper traffic routing between subnets.</li>
    </ul>
</body>
</html>
