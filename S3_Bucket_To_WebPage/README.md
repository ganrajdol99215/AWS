# 🌐 Hosting a Static Website Using AWS S3 Bucket

This guide explains how to create an S3 bucket, upload your static website data, and make it publicly accessible.

---

## 🗂️ What Type of Data Can You Store in S3?

You can store **any kind of digital file**, including:

- 📄 Documents: PDF, Word files, spreadsheets
- 🖼️ Images: JPG, PNG, GIF
- 🎥 Videos: MP4, MOV
- 🎵 Audio: MP3, WAV
- 🗄️ Backups: Database dumps, zip archives
- 📝 Logs: Text files from applications
- 💻 Software packages: `.zip`, `.tar.gz`
- 🌐 Static website files: HTML, CSS, JavaScript

---

## 📦 Bucket Types

- **General bucket:** No slashes in object keys.
- **Directory-style bucket:** Uses slashes (`/`) to visually organize keys.

✅ In this example, we are using a **General bucket**.

---

## 🏷️ Bucket Naming

- Bucket names **must be unique** across all AWS regions.
- Example: `ganrajs3webpage`

---

## 🔒 Understanding ACL (Access Control List)

**ACL** controls who can access your bucket and objects and what permissions they have:

**Permissions you can set:**
- `Read` – Download/List
- `Write` – Upload/Delete
- `Read ACP` – Read ACL
- `Write ACP` – Write ACL
- `Full Control`

**You assign these to:**
- The owner account
- Other AWS accounts
- Everyone (⚠️ Be cautious—public access)

---

## 🔄 Versioning

- Enables you to **maintain multiple versions** of an object (e.g., `V1`, `V2`).
- Useful for recovering from accidental overwrites or deletions.

---

## 🚀 Steps to Host Your Static Website

1. **Create the bucket**
   - Example: `ganrajs3webpage`

2. **Upload your website data**
   - You can **drag and drop files**, or use the console to upload.
   - 👉 **[You can download the webpage data from here](https://github.com/ganrajdol99215/AWS/tree/main/S3_Bucket_To_WebPage)**

3. **Enable Static Website Hosting**
   - Specify your `index.html` as the main page.
   - Optionally, set an error document (e.g., `error.html`).

4. **Configure Permissions**
   - Make your bucket publicly accessible:
     - Go to **Permissions**
     - Untick the "Block all public access" checkbox
     - Save changes
   - Allow ACL for the bucket and objects.

5. **Make Uploaded Objects Public**
   - Go to **Objects**
   - Select all files
   - Use **Actions > Make public using ACL**
   - Confirm changes.

6. **Update Files**
   - If you re-upload files:
     - **Make them public again** (or the updated webpage will not display)
     - If versioning is enabled, you will see multiple versions.

7. **Get the Website URL**
   - Go to **Properties tab**
   - Copy the **Bucket website endpoint URL**
   - Paste it into your browser to see your website live.

---

## 💡 Notes
- Always be cautious with public access settings.
- Consider enabling versioning to protect against accidental data loss.
- For production use, you may want to configure **CloudFront CDN** and **HTTPS**.

---

✅ **Enjoy hosting your static site on AWS S3!**
