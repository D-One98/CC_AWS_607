# CC_AWS_607
## ⚙️ Upload Protocol (VS Code Method)

If you are uploading your work, strictly follow these steps to avoid merge conflicts on the server.

### Step 1: Clone the Mainframe
1. Open VS Code.
2. Open a new Terminal (`Ctrl` + `` ` ``).
3. Run the following command to download the repository to your local machine:
   git clone https://github.com/D-One98/CC_AWS_607.git 
4.Open the cloned folder in VS Code (File > Open Folder).

**Step 2: Initialize Your Sector**
1. Inside the project structure, locate the students folder.
2. Inside students/, create a New Folder for yourself.
3. Name the folder strictly using your First Name and Last Name (e.g., Aditya_Mishra).

**Step 3: Transfer Data**
1. Drag and drop all your screenshots, project images, or videos directly into your specific folder inside the students directory.
2. Please use clear file names (e.g., output_1.png, frontend_view.jpg).

**Step 4: Synchronize and Push**
1. Open the VS Code terminal again. Make sure you are in the project folder.
2. Run these commands sequentially to authorize and upload your data:

**Bash**
# Stage your new files for upload
git add .

# Log your changes into the system with a clear message
git commit -m "docs: Added project screenshots for [Your Name]"

# Sync with the mainframe to prevent override conflicts with other students
git pull origin main

# Push your data to the remote server
git push origin main
