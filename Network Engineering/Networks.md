**Computer Network** : A computer network is a set of _nodes_ connected by _communication links_

**Fault Tolerance**

The ability to :

1. Continue working despite filter
2. Ensure there are no loss of service

**Scalability**

The ability to :

1. Grow based on needs
2. Have good performance after growth

**Quality of service**

The ability to :

1. Set priorities
2. Manage data traffic to reduce data loss

**Security**

The ability to prevent :

1. Unauthorized access
2. Misuse
3. Forgery

The ability to provide :

1. confidentiality
2. Integrity
3. Availability

---

**Data Flow:**

Simplex -> unidirectional -> one device send and the other receives -> keyboard

Half Duplex -> in both directions but not in the same time -> walkie-talkies

Full Duplex -> in both directions in the same time -> telephone line

**Protocol** is a set of rules to govern data communication

**Elements of protocol**

1. Message encoding
2. Message formatting and encapsulation
3. Message timing
4. Message size
5. Message delivery options

**Peer to peer network**

- No centralized administration
- All peers are equal
- Not scalable

**Client Server network**

- centralized administration
- request response model
- scalable
- server may be overloaded

---

**Classification of computer networks**

1. Local Area Network (LAN)
2. Metropolitan Area Network (MAN)
3. Wide Area Network (WAN)

**Local Area Network (LAN)**
within a limited are (e.g. school)

**Metropolitan Area Network (MAN)**
within a size of city

**Wide Area Network (WAN)**
large geographical area

---

**Network Topology**

Arrangement of nodes of a computer network

- Bus
- Ring
- Star
- Mesh
- Hybrid

---

**IP addressing**

IP address (IPV4)

- Every node in the computer network is identified with the help of IP
  address.
- Logical address.
- Can change based on the location of the device.
- Assigned by manually or dynamically.
- Represented in decimal and it has 4 octets (x.x.x.x).
- 0.0.0.0 to 255.255.255.255 (32 bits).

---

**MAC addressing**

- MAC stands for Media Access Control.
- Every node in the LAN is identified with the help of MAC address.
- IP Address = Location of a person.
- MAC Address = Name of the person.
- Every node in the LAN is identified with the help of MAC address.
- Physical address or Hardware Address.
- Unique.
- Cannot be changed.
- Assigned by the manufacturer.
- Represented in hexadecimal.
- Example: 70-20-84-00-ED-FC (48 bits).
- Separator: hyphen( − ), period( . ), and colon( : ).

| IP Address                                 | MAC Address                                |
| ------------------------------------------ | ------------------------------------------ |
| Needed for communication.                  | Needed for communication.                  |
| 32 bits.                                   | 48 bits.                                   |
| Represented in Decimal.                    | Represented in hexadecimal.                |
| _Router_ needs IP Address to forward data. | _Switch_ needs MAC address to forward data |
| Example: 10.10.23.56                       | Example: 70-20-84-00-ED-FC                 |

to view MAC and IP address, type the following command in `cmd`

```bash
ipconfig/all
```

---

**Switching techniques**

- Circuit Switching
- Message Switching
- Packet Switching
  - Datagram Approach
  - Virtual Circuit Approach

---

**Layering**

Decomposing the problem into more manageable components (layer)

**OSI Model**

- OSI -> Open System Interconnection

- It is not a protocol, it is only a guideline

- The purpose of the OSI model is to facilitate communication between
  different systems without requiring changes to the logic of the
  underlying hardware and software.

**OSI Layers**

- Application Layer
- Presentation Layer
- Session Layer
- Transport Layer
- Network Layer
- Data Link Layer
- Physical Layer

Acronym : Please Do Not Throw Sausage Pizza Away

![OSI model](assets/OSI%20model.jpeg)

- Application Layer : It enables the user to access the network resources

  - services :
    - File transfer and access management

- Presentation Layer : It is concerned with the syntax and semantics of the information exchanged between two systems

  - services :
    - Translation
    - Compression
    - Encryption

- Session Layer : It establishes, maintains and synchronizes the interaction among communication devices

  - services :
    - Dialog control
    - Synchronization

- Transport Layer : It is responsible for process to process delivery of the entire message

  - services :
    - Port addressing
    - Segmentation and reassembly
    - Connection control
    - End-to-end flow control
    - Error control

- Network Layer : It is responsible for delivery of data from the original source to the destination source

  - services :
    - Logical addressing
    - Routing

- Data Link Layer : It is responsible for moving data(frames) from one node to another

  - services :
    - Framing
    - Physical Addressing
    - Flow control
    - Error control
    - Access control

- Physical Layer : It is responsible for transmitting the bits over a medium. It also provides electrical and mechanical specifications

  - services :
    -  Physical characteristics of the media.
    -  Representation of bits.
    -  Data rate.
    -  Synchronization of bits.
    -  Line configuration.
    -  Physical topology.

