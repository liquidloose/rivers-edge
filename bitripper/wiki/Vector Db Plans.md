This is a solid architectural plan for a highly accurate, AI-powered civic archive. Here is the summary of the workflow for your Obsidian wiki.

---

## 🚀 Project: Automated Civic Archive & Fact-Checker

### 1. Data Ingestion & Vector DB Structure

- **Transcript Processing:** Segment 2,800 transcripts into **Time-Anchored Chunks**.
    
    - **Metadata:** Each chunk must store `meeting_id`, `date`, `speaker`, and `start_timestamp`.
        
- **Article Processing:** Store 2,800 AI-generated articles.
    
- **Indexing:** Use a Vector DB (ChromaDB, Pinecone, or Qdrant) with **Metadata Filtering** enabled to allow querying specific meetings by date.
    

### 2. Autonomous Timestamp Injection (The "Nested Loop")

To associate articles with the original record, run an autonomous script:

1. **Outer Loop:** Iterate through each **Article**.
    
2. **Inner Loop:** Split article into **Paragraphs**.
    
3. **Semantic Retrieval:** For each paragraph, query the Vector DB for the most similar transcript chunk, filtered by that article's `meeting_id`.
    
4. **LLM Verification:** Pass the paragraph + top transcript hits to an LLM to identify the **nearest preceding timestamp** where that topic began.
    
5. **Re-assembly:** Append the timestamp (e.g., `[01:22:10]`) to the paragraph and save the "Verified Article."
    

### 3. Community Notes & Semantic Validation

A system to allow users to suggest corrections without compromising factual integrity:

- **Direct Anchoring:** Users must select a specific paragraph to "Note," providing instant context.
    
- **AI Gatekeeper:** An LLM compares the user's note against the transcript in the Vector DB.
    
    - **Publish:** If the note identifies a factual discrepancy.
        
    - **Reject:** If the note is an opinion, spam, or factually unsupported.
        

### 4. Anti-Spam & Cost Protection ("The Tarpit")

To protect the API budget from "Denial of Wallet" attacks by bots, implement a tiered defense:

|**Layer**|**Type**|**Mechanism**|
|---|---|---|
|**Layer 1: Honeypot**|Hidden Field|An invisible field that, if filled, results in an immediate IP ban.|
|**Layer 2: Bot Bait**|Complex Math|A hidden, computationally difficult problem (e.g., _Calculate $7^{13} \pmod{191}$_) that only an AI bot would feel "compelled" to solve.|
|**Layer 3: CPU Tarpit**|MD5 Brute Force|A fake "Proof of Work" request (e.g., _Find a hash starting with 000000_) that forces the bot to burn its own CPU cycles before it can submit.|
|**Layer 4: Blackhole**|Silent Success|Return a fake `200 OK` status for blocked bots so they don't realize they've been caught and continue wasting their own resources.|

---

> **Key Principle:** The system uses the "Source Truth" (Transcripts) as a high-speed anchor for the "Narrative" (Articles), using AI to bridge the two while utilizing computational traps to protect the infrastructure.