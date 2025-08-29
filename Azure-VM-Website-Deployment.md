# Deploy Static Website on Azure VM with Nginx

Step-by-step guide to create an Azure VM and deploy a static website using Nginx.

## Prerequisites
- Azure Student Account
- SSH client
- Git installed

---

## Step 1: Create Azure VM

1. Go to [Azure Portal](https://portal.azure.com) → **Virtual Machines** → **Create**
2. **Configure Basic Settings:**
   - **Region**: Use your **Azure Student allotted region** (other regions will fail)
   - **Image**: Ubuntu Server 22.04 LTS
   - **Size**: B1s (Free tier)
   - **Authentication**: SSH public key
   - **Ports**: Allow SSH (22) and HTTP (80)
3. Click **Review + Create** → **Create**
4. **Download the .pem file** when prompted

---

## Step 2: Secure SSH Key

**Linux/Mac:**
```bash
mkdir -p ~/.ssh/azure_vm
mv ~/Downloads/mywebsite-vm_key.pem ~/.ssh/azure_vm/
chmod 400 ~/.ssh/azure_vm/mywebsite-vm_key.pem
```

**Windows:**
1. Create folder: `C:\Users\YourName\.ssh\azure_vm\`
2. Move downloaded `.pem` file to this folder

---

## Step 3: Connect via SSH

```bash
# Connect to VM (replace with your key name and VM IP)
ssh -i ~/.ssh/azure_vm/your-key.pem azureuser@YOUR_VM_PUBLIC_IP

# Example:
ssh -i ~/.ssh/azure_vm/myweb.pem azureuser@20.121.45.123
```

---

## Step 4: Install Nginx

```bash
# Update packages and install Nginx
sudo apt update -y
sudo apt install nginx -y

# Start and enable Nginx
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

---

## Step 5: Deploy Website

```bash
# Clone website repository
git clone https://github.com/anshuman018/Netflix-Static.git

# Copy files to web root
sudo rm -rf /var/www/html/*
sudo cp -r ~/Netflix-Static/* /var/www/html/

# Set permissions and restart Nginx
sudo chown -R www-data:www-data /var/www/html/
sudo systemctl restart nginx
```

---

## Step 6: Access Website

Open browser and go to: `http://YOUR_VM_PUBLIC_IP`

**Example:** `http://20.121.45.123`

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| VM creation fails | Use Azure Student allotted region |
| SSH permission denied | `chmod 400 ~/.ssh/azure_vm/your-key.pem` |
| Website not loading | Check port 80 is open in VM networking |
| Permission denied copying files | Use `sudo cp` command |

---

## Cleanup

To avoid charges: Azure Portal → Resource Groups → Delete your resource group

---

## Commands Reference

```bash
# Check Nginx status
sudo systemctl status nginx

# View logs
sudo tail -f /var/log/nginx/error.log

# Restart Nginx
sudo systemctl restart nginx
```

**Created for Azure Training**
