# IoMT Authentication Protocol Verification

## Overview

This repository contains the formal verification of an **Internet of Medical Things (IoMT) Authentication Protocol** using **CryptoVerif 2.11**. We formalize the IoMT authentication protocol proposed
by Lo et al. [1], incorporating post-quantum cryptography (NTRU). The protocol provides post-quantum secure authentication for medical IoT environments using **IND-CCA2 Key Encapsulation Mechanism (KEM)** with comprehensive security property verification.

## 🔒 Security Features

- **Post-Quantum Cryptography**: IND-CCA2 KEM for quantum-resistant encryption
- **Mutual Authentication**: Bidirectional verification between all parties
- **Forward Secrecy**: HKDF-based session key derivation
- **Data Integrity**: SUF-CMA MAC for device authentication
- **Identity Privacy**: Random Oracle Model for hidden identity (HID)
- **Medical Data Protection**: Multi-layer encryption for sensitive healthcare data

## 📋 Verified Security Properties

The protocol formally verifies **9 critical security properties**:

| Property | Type | Status | Description |
|----------|------|--------|-------------|
| **P1: Patient-to-SP Authentication** | Injective Correspondence | **PROVED** | Prevents replay attacks in patient registration |
| **P2: SP-to-Patient Authentication** | Correspondence | **PROVED** | Nonce-based response verification |
| **P3: Gateway Authentication** | Injective Correspondence | **PROVED** | Secure gateway registration |
| **P4: Doctor Authentication** | Existential Correspondence | **PROVED** | Doctor credential verification |
| **P5: Data Retrieval Authentication** | Correspondence |  **PROVED** | Secure device data transmission |
| **P6: Identity Hash Secrecy** | Random Oracle Model | **PROVED** | Protection of identity of entities |
| **P7: Medical Data Secrecy** | Secrecy | **PROVED** | Protection of sensitive medical data |
| **P8: Patient-Device Binding** | Correspondence | **PROVED** | Device ownership verification |
| **P9: Forward Secrecy** | Secrecy | **PROVED** | Session key independence after compromise |

## Protocol Architecture

### Entities
- **Patient (P)**: Medical service user with mobile device
- **Service Provider (SP)**: Healthcare service authority
- **Gateway (G)**: Network infrastructure component
- **Doctor (D)**: Medical professional with data access
- **Medical Device (MD)**: IoT sensors and monitoring equipment

### Cryptographic Primitives

| Primitive | Algorithm | Security Assumption | Purpose |
|-----------|-----------|-------------------|---------|
| **KEM** | IND-CCA2 KEM | Post-quantum security | Key encapsulation mechanism |
| **Symmetric Encryption** | AES-256-GCM | IND-CPA + INT-CTXT | Data confidentiality |
| **MAC** | HMAC-SHA256 | SUF-CMA | Device authentication |
| **Hash Function** | SHA-256 (ROM) | Random Oracle Model | Identity hiding (HID) |
| **Key Derivation** | HKDF-PRF | PRF security | Session key generation |

### Expected Output as Shown in Logs File
```
Proved query forall ni: nonce, h: hidValue, n: nid; inj-event(spReceivesReg(n, h, ni)) ==> inj-event(patientSendsReg(n, h, ni))
Proved query forall ni: nonce; event(patientReceivesResponse(ni)) ==> exists h: hidValue, n: nid; event(patientSendsReg(n, h, ni))
Proved query forall nj, nd: nonce; inj-event(spAcceptsGateway(nd, nj)) ==> inj-event(gatewayRegisters(nd, nj))
...
All queries proved.
```

## 🛡️ Security Analysis

### Cryptographic Assumptions
- **IND-CCA2 KEM**: Indistinguishability under adaptive chosen-ciphertext attack
- **AES-256-GCM**: Authenticated encryption with 256-bit security
- **HMAC-SHA256**: Pseudorandom function with collision resistance
- **Random Oracle Model**: Idealized hash function for identity hiding
- **HKDF-PRF**: Key derivation function based on HMAC

### Attack Resistance
✅ **Replay Attacks**: Prevented by injective correspondence properties  
✅ **Man-in-the-Middle**: Authenticated encryption with nonce verification  
✅ **Quantum Attacks**: Post-quantum KEM provides quantum resistance  
✅ **Forward Secrecy**: Session keys remain secure after long-term key compromise  
✅ **Data Tampering**: MAC-based integrity verification  
✅ **Identity Disclosure**: Hidden Identity (HID) protects patient privacy  

## 🧪 Testing & Validation

### Verification Results
- **Total Properties**: 9
- **Success Rate**: 100% (9/9 proved)
- **Verification Time**: ~25 seconds
- **Game Transformations**: 4 total games
- **Security Reductions**: Tight bounds (2^-126 to 2^-256 security)

### Known Limitations
- **Single Patient Process**: Uses implicit modeling for multi-party scenarios
- **CryptoVerif Constraints**: Sequential output operations require careful modeling
- **ROM Dependency**: Identity hashing relies on Random Oracle assumption

## 📚 References

1. **C. K. Man Lo, S. F. Tan and G. C. Chung, "Enhanced Authentication Protocol for Securing Internet of Medical Things with Lightweight Post-Quantum Cryptography,"** 2024 IEEE International Conference on Artificial Intelligence in Engineering and Technology (IICAIET), Kota Kinabalu, Malaysia, 2024, pp. 625-630, doi: 10.1109/IICAIET62352.2024.10730752.
2. **CryptoVerif**: [http://prosecco.gforge.inria.fr/personal/bblanche/cryptoverif/](http://prosecco.gforge.inria.fr/personal/bblanche/cryptoverif/)
3. **NTRU Cryptography**: [https://ntru.org/](https://ntru.org/)

## 📄 License

This research code is provided for academic and research purposes. Please cite appropriately if used in academic work.
