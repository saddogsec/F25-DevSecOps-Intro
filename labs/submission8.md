# Task 1 — Local Registry, Signing & Verification

### How signing protects against tag tampering and what “subject digest” means.

Signing protects against tag tampering by creating a digital signature tied to the exact data content, making any unauthorized modifications detectable.
The "subject digest" is the cryptographic hash of the signed data, serving as its unique identifier in the signature verification process.

# Task 2 - Attestations: SBOM (reuse) & Provenance

### How attestations differ from signatures

Attestations differ from signatures in that signatures confirm the authenticity and integrity of data, while attestations provide additional contextual information about the data’s origin, how it was produced, and under what conditions, offering more comprehensive verification beyond just integrity.

### What information the SBOM attestation contains

An SBOM attestation contains detailed information about software components: supplier name, component name, version, cryptographic hash of each component, unique identifiers, relationships between components, and the source of components, and metadata: the author, timestamp, and lifecycle stage when the SBOM was generated.

### What provenance attestations provide for supply chain security

Provenance attestations in supply chain security provide trustworthy records about the production process of software artifacts, including who built the software, when it was built, what source code or components were used, and the tools or environments involved. This helps ensure the software’s integrity and authenticity, aiding in detecting tampering or supply chain attacks.

# Task 3 - Artifact (Blob/Tarball) Signing

### Use cases for signing non-container artifacts (e.g., release binaries, configuration files)

The most common use case, is to distribute software to customers, so they can verify the authencity of received software, and ensure it wasn't tampered with.

### How blob signing differs from container image signing

Blob signing usually applies to individual binary large objects or files as standalone entities, while container image signing involves signing a structured image manifest and associated layers, which represent a full containerized application including metadata and multiple content pieces.
