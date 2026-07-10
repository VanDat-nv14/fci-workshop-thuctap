---
title: "Resource Cleanup"
date: 2026-04-17
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Resource Cleanup

After completing this hands-on workshop, to avoid incurring unexpected charges on your AWS account (especially for resources billed hourly such as NAT Gateways and Elastic IPs), please perform the following cleanup steps.

---

## Sequential Cleanup Process

### Step 1: Delete NAT Gateways

1. Navigate to the **VPC Dashboard → NAT Gateways**.
2. Select the NAT Gateway **`rookwork-nat`** created in step 5.3.
3. Click **Actions → Delete NAT gateway**.
4. Type `delete` to confirm and click **Delete**.
5. Wait for the status of the NAT Gateway to change to **Deleted** completely.

---

### Step 2: Release Elastic IPs

1. Navigate to the **VPC Dashboard → Elastic IPs**.
2. Select the Elastic IP addresses that were associated with the deleted NAT Gateway.
3. Click **Actions → Release Elastic IP addresses**.
4. Confirm to release the IPs from your AWS account (unassociated Elastic IPs incur charges if not released).

---

### Step 3: Delete the VPC and Dependent Resources

1. Navigate to the **VPC Dashboard → Your VPCs**.
2. Select the VPC **`rookwork-vpc`**.
3. Click **Actions → Delete VPC**.
4. Confirm by typing `delete` and click **Delete**.

> [!NOTE]
> When you delete a VPC, AWS will automatically delete all dependent network resources associated with it, including:
> - 4 Subnets (`rookwork-subnet-public1/2-...`, `rookwork-subnet-private1/2-...`)
> - 3 Route Tables (`rookwork-rtb-public`, `rookwork-rtb-private1/2-...`)
> - Internet Gateway (`rookwork-igw`)
> - VPC Endpoint (`rookwork-vpce-s3`)

---

### Step 4: Manually Delete Remaining Security Groups

If your Security Groups (`rookwork-alb-sg`, `backend-sg`, `rds-ec2-1`) were not automatically deleted with the VPC due to cross-references:
1. Navigate to the **VPC Dashboard → Security Groups**.
2. Select the above Security Groups.
3. Click **Actions → Delete security groups** and confirm.

---

## Post-Cleanup Verification

Ensure that the following sections are empty:
- NAT Gateways: No active gateways remain.
- Elastic IPs: No IP addresses are listed.
- VPCs: `rookwork-vpc` is no longer in the Your VPCs list.
- Security Groups: All security groups related to Rookwork have been deleted.
