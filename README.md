# Advanced Operating System ETHZ
This course is intended to give students a thorough understanding of design and implementation issues for modern operating systems, with a particular emphasis on the challenges of modern hardware features. We will cover key design issues in implementing an operating system, such as memory management, scheduling, protection, inter-process communication, device drivers, and file systems.

The report for our project can be found [here](https://github.com/bunert/network-stack-project/blob/main/AOS_Report.pdf). A rough overview is given below:

### Milestone 1
The first milestone was about implementing libmm, a library to manage and allocate physical memory for the rest of the system. When starting up, the init process collects all available RAM-caps and keeps track of its memory using a list of memory blocks.

### Milestone 2
The next goal was to be able to start a new process. The appropriate ELF image for the program must be found and parsed correctly, and some additional preparations related to the capability system must be made. Managing virtual address space was also introduced in this milestone which is required to spawn processes. In order to make sure a user can also map some memory at those locations we need to track the state of the MMU by building a shadow page table.

### Milestone 3
Spawning processes requires some sort of communication between processes. Inter-process communication will have at least two mechanisms, depending on the core of the source and destination. LMP (local message passing) and UMP (inter-core user-level message passing) are the two mechanisms used. But the LMP library was not intended for use by client programms, all requests would go through our AOS RPC library for sending specific requests.

### Milestone 4
In this chapter we added page fault handling where we veryify the exception (e.g. address is in the expected range), acquire physical memory and map the new physical memory to that address. In this section we also tested our implementation with threads. Threads share a single address space, consequently a single page table, which could lead to a race condition, where two or more threads fault inside the same area.

### Milestone 5
Starting a secondary core was the next task. When starting a new core, each core has a memory manager service running, the same as on core 0. But before we boot up a second core, we request half of the available memory from our current memory manager on core 0. After booting the second core the next goal was to enable communication beween cores using shared memory. 

### Milestone 6
In this milestone we refactorered the entire message passing system. We included a message passing (MP) layer, which we introduced between RPC and LMP/UMP. 

### Milestone 7
The last milestone includes five individual projects: Shell, Filesystem, Networking, Nameserver and Capabilities revisited. I focus on the network stack because I was responsible for that part.

The goal was to develope a simple network stack on our running system. The starting point was a simple driver polling on the receive queue where the processing of the network stack begins. Our network stack receives the unprocessed raw packets from the receiver queue and processes them according to their content. By managing the Ethernet frame itself, we also need to know the MAC address of the destination to which we want to send a packet, which is where the Address Resolution Protocol (ARP) comes into play. In Addition, we have built basic support for the Internet Protocol (IP), User Datagram Protocol (UDP) and Internet Control Message Protocol (ICMP). 
