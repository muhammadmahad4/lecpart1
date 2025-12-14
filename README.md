The following notes are a comprehensive, exam-focused summary of the provided materials, organized by topic and structured for maximum clarity and retention.

---

# **CS 4394 Information Security and Management: Exam-Focused Revision Notes**

## **1. Fundamentals of Information Security**

Information security is fundamentally defined by three major aspects, often called the CIA Triad, which represent the primary goals of any security system. Beyond the triad, other aspects like authentication and non-repudiation are also crucial for a complete security posture. Mechanisms such as cryptography, digital signatures, access control, firewalls, passwords, and certificates are used to protect one or more of these major aspects.

### 1.1 Aspects of Security (The CIA Triad)

| Aspect | Definition | Key Question |
| :--- | :--- | :--- |
| **Confidentiality** | Protection of information from unauthorized disclosure. | **Who can read information?** |
| **Integrity** | Protection of information from unauthorized modification, writing, or generation. | **Who can write, modify or generate information?** |
| **Availability** | Ensuring that resources are accessible and usable when needed. | **Are resources available when needed?** |

### 1.2 Other Security Aspects

* **Authentication:** The process of establishing identity. This involves making sure that a given entity (with whom you are interacting) is who you believe it to be.
* **Non-repudiation:** The ability to prevent an individual from denying their action.

### 1.3 Metapolicy and Policy

A **Metapolicy** is the high-level goal or philosophical principle of a security model (e.g., "constrain the flow of information among different security levels" for confidentiality). A **Security Policy** is a specific set of rules derived from the metapolicy that dictates how the system is governed.

* The metapolicy is often too general to provide adequate guidance, which is why a concrete security policy is needed.
* The goal of a security policy is to restrict the set of authorized actions (accesses) to ensure the system does not enter an unauthorized state.

### 1.4 Trusted Computing Base (TCB)

The **Trusted Computing Base (TCB)** consists of all the parts of a system (hardware, firmware, software) that are essential to enforce a security policy.

| Benefits of TCB | Limitations of TCB |
| :--- | :--- |
| Only need to focus on components in the TCB to implement a security policy. | The TCB is "trusted" but may not be "secure". Bugs or vulnerabilities may still occur inside it. |
| Easier to reason about the security of a large system. It may be used as an abstraction of a subsystem. | Some applications need to be trusted *outside* the TCB to be efficient (e.g., database systems managing their own storage). |

***

## **2. Access Control Policies and Mechanisms**

Access Control Policies introduce rules to control what accesses (actions) **subjects** may take with respect to **objects**.

### 2.1 Subjects, Objects, and Access

* **Subjects:** Active entities in the system (e.g., humans, processes).
* **Objects:** Passive entities in the system that contain or receive information (e.g., files, folders, devices).
* **Accesses/Actions:** Operations a subject can perform on an object (e.g., read, write).

### 2.2 Mandatory vs. Discretionary Access Controls (MAC vs. DAC)

This is a key distinction in how access rules are enforced.

| Feature | Mandatory Access Controls (MAC) | Discretionary Access Controls (DAC) |
| :--- | :--- | :--- |
| **Enforcement** | Rules are enforced on *every attempted access*. | Rule enforcement may be *waived or modified by some users*. |
| **Control** | Not at the discretion of any system user. | Typically controlled by the object's owner. |
| **Example** | The Bell-LaPadula (BLP) Model. | Unix file protection system, where the file's owner can modify protections. |

### 2.3 Access Control Matrix (ACM)

An **Access Control Matrix (ACM)** is a general concept that can represent *any* access control policy.

* The matrix shows explicitly what accesses are allowed for each **subject/object pair**.
* For large, realistic systems (like a BLP system), the matrix would be **huge** and is thus often **implicit** in the access control rules (e.g., the Simple Security Property and *-Property). Access permissions can be computed **on the fly**.

| Storage Alternative | Name | Description |
| :--- | :--- | :--- |
| Storing permissions with objects | **Access Control Lists (ACLs)** | Each object has a list naming subjects/groups and their allowed permissions. |
| Storing permissions with subjects | **Capabilities** | Each subject maintains a collection of pairs `<Object, Access>`. |
| Computing permissions on the fly | **Global Rules (e.g., BLP)** | Permissions are calculated based on system rules (like BLP's security properties). |

### 2.4 Role-Based Access Control (RBAC)

RBAC is a widely used security framework, claimed to be especially appropriate for commercial settings.

* **Mechanism:** RBAC associates permissions with **functions/jobs/roles** within an organization, unlike other policies that assign permissions directly to subjects. A role is a set of subjects who are viewed as interchangeable for the purpose of access control.
* **Key Advantage:** RBAC is generally more flexible and easy to administer than standard access control policies. For example, everyone in the role of 'teller' has the same permissions.
* **Benefit:** Permissions are appropriate to the organization (e.g., "open an account" rather than "read a file"). It also allows a subject to transition between roles without changing identities, recognizing that a subject often has various functions.
* **Role Engineering/Mining:** Defining roles and needed permissions; *Role mining* involves analyzing existing roles/permission assignments to determine optimal structure.

| Access Control Type | Authorization Focus |
| :--- | :--- |
| **Role Authorization** | Authorization granted to an entity (a subject) to assume a particular **role**. |
| **Transaction Authorization** | Authorization granted to a **role** to execute a particular **transaction** (a set of related operations). |

***

## **3. Confidentiality Models: Bell-LaPadula (BLP) Model**

The Bell-LaPadula (BLP) Model is an example of an **access control policy** and specifically a **Mandatory Access Control (MAC)** system. It is designed to enforce the metapolicy of **Confidentiality** by constraining the flow of information among different security levels.

### 3.1 Multi-Level Security (MLS) and Labels

* **Concept:** Not all information is equally sensitive, so information is categorized and parceled out into separate containers (objects).
* **Security Level:** Each subject $S$ is assigned a clearance level ($L_S$, $C_S$) and each object $O$ is assigned a sensitivity level ($L_O$, $C_O$).
    * $L$: Hierarchical level (e.g., Top Secret ($T$) > Secret ($S$) > Confidential ($C$) > Unclassified ($U$)).
    * $C$: Category set (e.g., $\text{\{NATO, A, B\}}$).
* **Dominance:** A security level $(L_1, C_1)$ is said to **dominate** $(L_2, C_2)$, denoted $(L_1, C_1) \geq (L_2, C_2)$, if and only if:
    1.  $L_1 \geq L_2$ (the hierarchical level of 1 is greater than or equal to 2).
    2.  $C_1 \supseteq C_2$ (the category set of 2 is a subset of the category set of 1).

### 3.2 The Bell-LaPadula Rules

BLP restricts read and write access to enforce confidentiality (i.e., **no information flows from a higher level to a lower level**).

| BLP Property | Rule (Conditions for Access) | Security Principle Enforced | Interpretation |
| :--- | :--- | :--- | :--- |
| **Simple Security Property (No Read Up)** | A subject $S$ can read an object $O$ only if $L_S \geq L_O$. | Prevents a lower-clearance subject from reading a higher-sensitivity object. | Subjects can only read content at or below their own security level. |
| ***-Property (Star Property - No Write Down)** | A subject $S$ can write to an object $O$ only if $L_S \leq L_O$. | Prevents a higher-clearance subject from writing (downgrading) information to a lower-sensitivity object. | Subjects can create content only at or above their own security level. |

A subject must satisfy both the Simple Security Property and the *-Property for a combined read/write access.

* **Condition for Read and Write Access:** For a subject $S$ to have both read and write access to an object $O$, it must be true that the subject's clearance level is equal to the object's sensitivity level: $L_S = L_O$.

### 3.3 Non-interference (NI)

**Non-interference** is a security property that states if security demands that a high-clearance subject ($S_H$) must never communicate with a low-clearance subject ($S_L$), there shouldn't be anything that $S_H$ can do that has effects visible to $S_L$.

* **Relationship with BLP:**
    * It is **possible** to turn any MLS policy (like BLP) into an NI policy.
    * It is **NOT true** that any NI policy can be reformulated into an MLS policy.
    * A system that satisfies BLP does **NOT necessarily** satisfy NI. For instance, a system that uses a firewall to separate INTERNET ($L_L$) and LAN ($L_H$) satisfies BLP ($L_H \geq L_L$), but a channel from INTERNET into LAN would violate an explicit NI rule to refuse that channel.
* **Difficulty in Proving NI:** It is difficult to prove non-interference for realistic systems because:
    1.  Interferences are common in the real world.
    2.  Interferences often involve low-level system attributes that are scattered, diverse, and hard to capture or represent in a programming language.
    3.  Some interferences are benign (e.g., using a cache to speed up data retrieval).

### 3.4 Covert Channels

**Definition:** A **covert channel** is a path for the illegal flow of information between subjects within a system, utilizing system resources that were *not designed* to be used for inter-subject communication.

* Information flows in violation of the policy's rules.
* The channel uses system resources not intended for information transfer, making it hard to detect.
* **Implicit Channel:** One that uses the control flow of a program.
* **Covert Storage Channel:** Conditions for existence:
    1.  Both sender and receiver must have access to some attribute of a shared object.
    2.  The sender must be able to modify the attribute.
    3.  The receiver must be able to detect the modification.
* **Characteristics of a Covert Channel:**
    * **Existence:** Is a channel present or not?
    * **Bandwidth:** How much information can be transmitted per second?
    * **Noiseless/Noisy:** Can the information be transmitted without error?
* **Infeasibility of Elimination:** It is infeasible to eliminate every potential covert channel because:
    1.  Covert channels use system resources not intended for information transfer, making it hard to detect every one.
    2.  Protection is inherently costly, especially when implementing countermeasures for as many unintended channels as possible.
* **Detection:** **Kemmerer's Shared Resource Matrix Methodology** provides a systematic way to investigate potential covert channels, but its effective use requires extensive knowledge of the system's semantics and implementation.

***

## **4. Integrity Models**

Integrity is a fuzzier notion than confidentiality and is more context-dependent. In many commercial settings, integrity concerns are frequently **more important** than confidentiality concerns.

### 4.1 The Integrity Metapolicy

The integrity metapolicy aims to prevent bad (low integrity) information from "tainting" good (high integrity) information. Alternatively, it dictates: **information should NOT flow up in integrity**.

* This is achieved by preventing a **low integrity subject** from writing bad information into a **high integrity object** (no "write up" in integrity).
* It also prevents a **high integrity subject** from reading bad information from a **low integrity object** (no "read down" in integrity).

### 4.2 Biba's Integrity Models

Ken Biba (1977) proposed three integrity access control policies, all of which associate integrity labels with subjects and objects analogous to clearance levels in BLP. The main difference among them is the amount of trust invested in subjects.

#### Strict Integrity Policy (The Biba Model)

The Strict Integrity Policy is a **mandatory integrity access control policy** that is the **dual of BLP**. It places **very little trust** in subjects and aims to keep information from flowing up in integrity.

| Biba Property | Rule (Conditions for Access) | Security Principle Enforced | Interpretation |
| :--- | :--- | :--- | :--- |
| **Simple Integrity Property (No Read Down)** | Subject $s$ can read object $o$ only if $i(s) \leq i(o)$. | Prevents a subject's integrity from being tainted by reading bad (lower integrity) information. | A subject can only read objects at its own integrity level or **above**. |
| **Integrity \*-Property (No Write Up)** | Subject $s$ can write to object $o$ only if $i(o) \leq i(s)$. | Prevents a subject from tainting more reliable (higher integrity) information by writing into it. | A subject can only write objects at its own integrity level or **below**. |

* **Duality with BLP:** The Biba Strict Integrity rules are the mirror image (dual) of the BLP rules (e.g., BLP's "no read up" corresponds to Biba's "no read down," and BLP's "no write down" corresponds to Biba's "no write up").
* **Combining Policies:** Since confidentiality and integrity are orthogonal, one could use both BLP (for confidentiality) and Biba's Strict Integrity (for integrity). An access is allowed *only if* it is allowed by **both** sets of rules.

#### Low Water Mark (LWM) Integrity Policy

A **water mark policy** is one where an attribute monotonically floats up (high water mark) or down (low water mark), but may be "reset" at some point.

* **Rules:**
    1.  If subject $s$ reads object $o$, then its new integrity level is $i'(s) = \min(i(s), i(o))$.
    2.  Subject $s$ can write to object $o$ only if $i(o) \leq i(s)$ (Integrity \*-Property is retained).
* **Trust Assumption:** The LWM policy is **less trusting** of the subject because its integrity level falls if it reads low-integrity information.
* **Issue:** A potential issue is **label creep**, where the subject levels monotonically decrease, making it so that soon no subject will be able to access objects at high integrity levels. This results in an overly conservative analysis.

#### Ring Policy

The Ring Policy focuses on direct modification and is **more trusting** of the subject, assuming the subject can properly filter the information they read.

* **Rules:**
    1.  **Any subject can read any object**, regardless of integrity levels.
    2.  Subject $s$ can write to object $o$ only if $i(o) \leq i(s)$ (Integrity \*-Property is retained).
* **Trust Assumption:** The assumption is that a subject can read low-integrity information without being compromised and can properly filter out the "bad" information.

### 4.3 Clark-Wilson Integrity Model (CWM)

The Clark-Wilson Model identified integrity concerns claimed to be of particular relevance within commercial environments, focusing on separation of duty, transactions, consistency, authentication, and audit.

#### Fundamental Concerns (Integrity Metapolicy)
Clark and Wilson claimed the following are four fundamental concerns of any reasonable commercial integrity model:
1.  **Authentication:** The identity of all users must be established.
2.  **Separation of Duty:** Duties must be separated (e.g., no single person has control over all steps in a critical process).
3.  **Auditing:** Transactions must be logged and checked for accuracy.
4.  **Well-Formed Transactions:** Transactions must move the system from one consistent state to another.

#### Entities
* **Constrained Data Items (CDIs):** Data items whose integrity is protected.
* **Unconstrained Data Items (UDIs):** Data items not covered by integrity rules, though they may eventually be transformed into CDIs.
* **Transformation Procedures (TPs):** Certified programs (transactions) that manipulate CDIs, ensuring the system remains in a valid state.
* **Integrity Verification Procedures (IVPs):** Procedures that certify CDIs are in a valid state.

#### Policy Rules (Certification $\&$ Enforcement)

CWM rules are split into **Certification Rules (C)**, which specify requirements the application system should uphold as transactions happen, and **Enforcement Rules (E)**, which specify requirements that should be supported by the underlying system's protection mechanisms.

| Rule Type | Rule ID | Description |
| :--- | :--- | :--- |
| **Certification (C)** | C1 | All IVPs must ensure that CDIs are in a valid state when the IVP is run. |
| **Certification (C)** | C2 | All TPs must be certified as integrity-preserving. |
| **Certification (C)** | C3 | The assignment of TPs to users must satisfy separation of duty. |
| **Certification (C)** | C4 | The operation of TPs must be logged (append enough information to an append-only CDI, the log). |
| **Certification (C)** | C5 | TPs executing on UDIs must result in valid CDIs. |
| **Enforcement (E)** | E1 | Only certified TPs can manipulate CDIs. |
| **Enforcement (E)** | E2 | Users must only access CDIs by means of TPs for which they are authorized. |
| **Enforcement (E)** | E3 | The identity of each user attempting to execute a TP must be authenticated. Authentication is required before manipulation of CDIs (i.e., before using TPs). |
| **Enforcement (E)** | E4 | Only the certifier of a TP may change the list of entities associated with that TP. |

* **Allowed Relations:** Permissions are encoded as a set of triples: $(\text{user}, \text{TP}, \{\text{CDI set}\})$. This states that the **user** is authorized to perform the transaction procedure **TP** on the given set of **CDIs**.

***

## **5. Specialized Access Control Models**

These models are crafted to address very specific, contextual security concerns.

### 5.1 Brewer-Nash Chinese Wall Policy (CW)

The Chinese Wall Policy is an access control policy designed to address the specific concern of **conflicts of interest** by a consultant or contractor. Accesses are **sensitive to the history of past accesses**.

#### Entities
* **Objects:** Objects (e.g., files) contain information about only one company.
* **Company Dataset (CD):** Contains all objects concerning a particular company, denoted $CD(o)$.
* **Conflict of Interest Classes (COI):** Contain datasets of companies that are in competition. Each object belongs to exactly one COI class.

#### Rules
1.  **CW-Simple Security Rule (Reading Restriction):** A subject $s$ can read an object $o$ only if the subject has not previously accessed *any* object in a Company Dataset ($CD$) that is in **conflict** with $CD(o)$.
    * **Interpretation:** If a subject reads an object in a COI class, they can never read another $CD$ in that same COI class.

2.  **CW-**-**Property (Writing Restriction):** A subject $s$ can write to an object $o$ if and only if **both** of the following hold:
    * The CW-Simple Security Rule permits $s$ to read $o$.
    * For all unsanitized objects $o'$, if $s$ can read $o'$, then $CD(o') = CD(o)$.
    * **Interpretation:** A subject can write to an object $o$ only if all the (unsanitized) objects the subject can read belong to the **same Company Dataset** as $o$. This rule prevents information leakage: if a subject could read from one CD (e.g., Bank 1) and write to a second CD (e.g., Gas), and a third subject could then read from the second CD (Gas), the third subject would indirectly get Bank 1's information, which is a conflict of interest.

### 5.2 Confidentiality and Integrity in Structured Systems (CISS)

CISS is a policy crafted to address the specific concerns of **medical records**, combining both integrity and confidentiality. Key concerns include patient confidentiality, authentication of records, and integrity. CISS focuses on the **objects being accessed**.

#### Access Principles
1.  **Access Control Lists (ACLs):** Each medical record has an ACL naming the individuals or groups who may **read and append** information. The system must restrict access to those on the ACL (e.g., clinicians).
2.  **Responsible Clinician:** One of the clinicians on the ACL must have the right to add other clinicians to the ACL. This person has overall responsibility for patient care.
3.  **Patient Notification:** The responsible clinician must notify the patient of the names on the ACL whenever it is changed.

#### Confinement Principle (Integrity/Confidentiality)
Information from one medical record (Record 1) may be appended to a different medical record (Record 2) **if and only if** the access control list of the second record ($ACL_2$) is a **subset** of the access control list of the first record ($ACL_1$).

* **Goal:** This principle prevents information from leaking to unauthorized users by ensuring that all users who can access the new combined information were already authorized to access the source information.
* **Comparison to BLP:** The Confinement Principle imposes a lattice structure similar to Bell-LaPadula.

#### CISS as an Instantiation of Clark-Wilson
CISS contains specific instantiations of the abstract Clark-Wilson policy:
* **CDIs:** Medical records.
* **TPs:** Functions updating records and access control lists.
* **Auditing (C4):** Making all records append-only and notifying the patient when the ACL is changed.

***

## **6. Tutorial Examples and Solutions**

These examples illustrate the concepts in a practical, exam-style context.

### 6.1 Tutorial 1: Core Concepts & BLP

| Q\# | Question | Answer/Explanation |
| :--- | :--- | :--- |
| **1** | Which is most important: confidentiality, integrity, availability? Justify your answer. | **It all depends on the context**. For e-banking, **integrity** is crucial (accurate, untampered financial transactions). For personal data protection, **confidentiality** is more important (protecting data from unauthorized access). For continuously relied-upon services (like cloud files), **availability** is critical. |
| **2** | What is the connection of integrity and authentication? Can you illustrate with examples? | **Connection:** Authentication is a way to **enforce integrity**. Alternatively, **authenticity is a special case of integrity**. **Example:** Submitting an original transcript to a university. The transcript is sealed in an envelope with a stamp. The university uses the seal/stamp (authentication) to verify the document's integrity (that it hasn't been altered from the reference version). |
| **8** | State informally what the \*-Property (BLP) says. | **\*-Property:** A subject $S$ can write to an object $O$ only if $L_S \leq L_O$ ("write up"). Users can create content only at or above their own security level. |
| **9** | What must be true for a subject to have both read and write access to an object (BLP)? | The subject's clearance level ($L_S$) must be **equal to** the object's sensitivity level ($L_O$). Both $L_S \geq L_O$ (read) and $L_S \leq L_O$ (write) must be satisfied. |
| **16** | If a computer system satisfies BLP, does it necessarily satisfy non-interference (NI)? | **No**. BLP permits the existence of covert channels that violate non-interference (e.g., in a firewall scenario where an INTERNET user can signal to a LAN user despite BLP being satisfied). |

### 6.2 Tutorial 2: Integrity & Specialized Models

| Q\# | Question | Answer/Explanation |
| :--- | :--- | :--- |
| **1** | Give examples of information that is highly reliable with little sensitivity and information that is not so highly reliable but with greater sensitivity. | **Highly reliable with little sensitivity:** Published peer-reviewed papers. **Not highly reliable with greater sensitivity:** Military intelligence from a double spy. |
| **4** | What do Biba's three integrity policies (Strict, Low water mark, and Ring policy) have in common? | They all assume that integrity labels are associated with subjects and objects. They all share the **Integrity \*-Property** (no write up): Subject $s$ can write to object $o$ only if $i(o) \leq i(s)$. |
| **7** | What is the purpose of the four fundamental concerns of Clark and Wilson? | They define a minimal set of requirements for **commercial integrity models**, focusing on: **Authentication**, **Separation of Duty**, **Auditing**, and **Well-Formed Transactions**. |
| **9** | In the example conflict classes $(\{\text{Ford, Chrysler, GM}\}, \{\text{HSBC, Standard Charter, Citicorp}\}, \{\text{Microsoft}\})$, if you accessed a file from General Motors, then subsequently accessed a file from Microsoft, will you then be able to access another file from GM? | **Yes**. You are free to access files from companies in any other **Conflict of Interest (COI) class**. Accessing GM (COI 1) does not prevent you from accessing Microsoft (COI 3), and neither access prevents you from re-accessing another file from GM (since it is in the same CD/COI as your first access). |

### 6.3 Tutorial 3: Access Control & Specialized Concepts

| Q\# | Question | Answer/Explanation |
| :--- | :--- | :--- |
| **1** | Suppose a subject has access on an object called $X$ in the company dataset $g$. What else can also be accessed according to the simple security rule of the Chinese wall model? | The subject can access **any object in company dataset $g$** and **any object in COI classes $B$ and $C$**. The subject is blocked from reading any objects in company dataset $f$ or $h$ (which are in the same COI class $A$ as $g$). |
| **2** | If a patient is mentally incompetent... Which principle applies, and how is this handled? | **c. Access Principle 2; the responsible clinician can add the guardian to the access control list**. Access Principle 2 states that one of the clinicians (the responsible clinician) on the ACL must have the right to add other clinicians (or a guardian acting on the patient's behalf) to the ACL. |
| **3** | What benefits are there in associating permissions with roles, rather than subjects? | **RBAC Advantages:** It is generally more flexible than standard access control policies. It is easy to administer, as everyone in a role has the same permissions. Permissions are appropriate to the organization ("open an account" rather than "read a file"). |
| **5** | Why would one not want to build an explicit ACM for an access control system? | For a realistic system with many subjects and objects, the ACM would be **huge**. The matrix can be implicit in the rules, allowing access permissions to be computed **on the fly**. |
| **6** | Name, in order, the ACM alternatives for storing permissions with objects, storing permissions with subjects and computing permissions on the fly. | **Access Control Lists (ACLs)** (with objects) $\rightarrow$ **Capabilities** (with subjects) $\rightarrow$ **Global Rules** (computing on the fly). |

***

## **7. Key Takeaways / Most Important Points for Exam**

### Definitions and Core Concepts
* **CIA Triad:** Confidentiality (No Read), Integrity (No Write/Modify), Availability (Ready when needed). Integrity is often more important than confidentiality in commercial settings.
* **Access Control Matrix (ACM):** General representation of any policy, but often too huge to be explicit in large systems, so permissions are often computed on the fly via Global Rules.
* **MAC vs. DAC:** MAC (Mandatory) rules are enforced on *every* access and cannot be waived (e.g., BLP). DAC (Discretionary) rules can be modified by some users (e.g., Unix file owners).
* **Covert Channel:** Unauthorized information flow using system resources *not* designed for communication.

### Confidentiality (BLP) Rules (No Information Flow Down)
* **Simple Security Property (No Read Up):** $L_S \geq L_O$ (Subject must dominate or equal object level to read).
* ***-Property (No Write Down):** $L_S \leq L_O$ (Subject must be dominated by or equal object level to write).
* **Read/Write Access:** Requires $L_S = L_O$.

### Integrity (Biba) Rules (No Information Flow Up)
* **Strict Integrity is the Dual of BLP**.
* **Simple Integrity Property (No Read Down):** $i(s) \leq i(o)$ (Subject must be dominated by or equal object integrity to read).
* **Integrity \*-Property (No Write Up):** $i(o) \leq i(s)$ (Object must be dominated by or equal subject integrity to write).
* **LWM Policy:** Subject integrity level *falls* to the minimum of its current level and the object it reads: $i'(s) = \min(i(s), i(o))$.

### Specialized Model Rules
* **Clark-Wilson Concerns (for Commercial Integrity):** Authentication, Separation of Duty, Auditing, Well-Formed Transactions. Key entities are CDIs, UDIs, TPs, and IVPs.
* **Chinese Wall Simple Security (Read Rule):** Cannot read an object if you have accessed a conflicting Company Dataset (CD) in the same Conflict of Interest (COI) class.
* **Chinese Wall \*-Property (Write Rule):** Can only write to $O$ if you can read $O$ *and* all other unsanitized objects you can read are in the *same* Company Dataset as $O$. (Prevents information flow/conflict of interest via writing).
* **CISS Confinement Principle:** Appending to a record is allowed only if the destination record's ACL is a **subset** of the source record's ACL. (Prevents leakage to unauthorized users).
