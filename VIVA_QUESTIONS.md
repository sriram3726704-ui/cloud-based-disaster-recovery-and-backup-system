# CloudVault: College Project Viva & Faculty Q&A Guide

This guide contains the most common technical, conceptual, and architectural questions college faculty and external examiners ask during project demonstrations, along with complete, high-scoring answers.

---

### Q1: What is the primary objective of the CloudVault project?
**Answer:**  
"The primary objective of CloudVault is to implement a cloud-native disaster recovery and automated backup system. In the event of primary hardware failure, ransomware infection, or accidental data corruption, CloudVault ensures business continuity by maintaining encrypted, geographically isolated cloud replicas. It achieves near-zero Recovery Point Objective (RPO) and a sub-5-second Recovery Time Objective (RTO) using Google Firebase's infrastructure."

---

### Q2: What is the difference between RPO and RTO?
**Answer:**  
- **RPO (Recovery Point Objective):** Represents the maximum acceptable age of files that must be recovered from backup storage for normal operations to resume (i.e., "How much data can we afford to lose in terms of time?"). In CloudVault, files are synchronized immediately upon modification or upload, keeping RPO to minutes or seconds.
- **RTO (Recovery Time Objective):** Represents the maximum acceptable duration of time that can elapse after a disaster before systems and files are fully restored (i.e., "How long does it take to recover?"). CloudVault achieves an RTO under 5 seconds by streaming files directly from Firebase Cloud Storage via high-speed CDN edges.

---

### Q3: Explain the 3-2-1 Backup Rule and how CloudVault adheres to it.
**Answer:**  
"The 3-2-1 Backup Rule is an industry standard for data protection:
1. **3 Copies of Data:** Keep 1 primary copy and 2 backup replicas.
2. **2 Different Storage Media:** For example, local hard drive/SSD and cloud object storage.
3. **1 Offsite Copy:** At least one copy must be located in a physically separate facility.
CloudVault handles the off-site and secondary media tiers by automatically replicating data into Google Cloud multi-region data centers, ensuring protection against physical facility disasters (e.g., power loss, flood, fire)."

---

### Q4: Why did you choose Firebase instead of a traditional SQL database and server?
**Answer:**  
"We selected Firebase for three key architectural advantages:
1. **Serverless Scalability:** Cloud Firestore and Cloud Storage scale automatically without requiring physical server maintenance, OS patching, or provisioning virtual machines.
2. **Built-in Global Durability & Redundancy:** Firebase Cloud Storage is built on Google Cloud Storage, which delivers **99.999999999% (11 9's)** annual durability and multi-region replication.
3. **End-to-End Security with Declarative Rules:** Instead of writing complex backend authorization endpoints, Firebase enforces server-side declarative security rules (`firestore.rules` and `storage.rules`) right at the database and storage engine level."

---

### Q5: How is multi-tenant security and user isolation enforced? Can User A see or delete User B's files?
**Answer:**  
"No, User A can never access or delete User B's files. Security is enforced through a two-tier defense model:
1. **Client-Side Enforcement:** Each query to Firestore includes a filter `where('userId', '==', currentUser.uid)`.
2. **Server-Side Enforcement (The True Security Barrier):** Even if an attacker manipulates the client-side JavaScript, Firebase's backend executes `firestore.rules` and `storage.rules`. These rules inspect the cryptographically signed JWT authentication token and verify that `request.auth.uid == userId` and `resource.data.userId == request.auth.uid`. Any request to access another user's files is rejected with a `403 Permission Denied` error by the cloud servers."

---

### Q6: Why can't a deleted file be restored in CloudVault?
**Answer:**  
"When a user permanently deletes a backup, CloudVault calls Firebase Storage's `deleteObject()` API. This physically purges the encrypted object blocks from the cloud bucket to prevent unwanted storage costs and adhere to data privacy standards such as GDPR's 'Right to be Forgotten'. In disaster recovery terminology, once the primary cloud replica is explicitly destroyed without a secondary immutable snapshot or versioning bucket policy, it is non-recoverable by design."

---

### Q7: What validation steps are performed before a file is accepted for backup?
**Answer:**  
"CloudVault performs both client-side and server-side validation:
1. **File Size Limit:** Files are capped at 25 MB to prevent quota exhaustion and Denial of Service (DoS) attacks.
2. **MIME & Extension Whitelisting:** CloudVault restricts uploads to known file formats (PDF, DOCX, DOC, JPG, PNG, TXT, CSV, ZIP, XLSX, PPTX) to prevent malicious script execution.
3. **Security Rule Verification:** Firebase Storage security rules re-check `request.resource.size < 25 * 1024 * 1024` on the server before accepting byte writes."

---

### Q8: What is the purpose of the Activity Audit Log?
**Answer:**  
"In compliance standards like ISO 27001, HIPAA, and SOC 2, an immutable audit trail is required for all data lifecycle events. CloudVault logs every upload, download, disaster drill, and deletion event with a timestamp, user ID, target file, and execution status into the `activity_logs` collection in Firestore. Furthermore, the Firestore security rules declare `allow update, delete: if false;`, ensuring that audit logs are append-only and cannot be altered or covered up."

---

### Q9: What happens if the network drops in the middle of a file upload?
**Answer:**  
"CloudVault utilizes Firebase Storage's `uploadBytesResumable()` API rather than basic static upload calls. Resumable uploads split the file stream into chunked byte ranges and maintain snapshot state (`snapshot.bytesTransferred`, `snapshot.totalBytes`). If network connectivity fluctuates, the client monitors state transitions and surfaces error notifications cleanly without corrupting the cloud repository."

---

### Q10: What future enhancements could be added to CloudVault?
**Answer:**  
"Possible future extensions include:
1. **Client-Side End-to-End Encryption (E2EE):** Encrypting file bytes with AES-GCM in the browser before upload so even cloud providers cannot read the plaintext.
2. **Automated Scheduled Backups:** Using Web Workers and Background Sync API to take automatic snapshot backups at designated intervals.
3. **Differential / Incremental Delta Backups:** Uploading only modified byte blocks rather than full files to conserve network bandwidth."
