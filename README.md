# Smart-Grid-Privacy
This repository contains the application written for a project in *ICT Support for Adaptiveness and (Cyber)security in the Smart Grid*.

### Background
When we consider privacy issues in the smart grid, there are some important aspects that need to be considered and that cannot be overseen. One of which is that the electricity producer needs accurate data about how much electricity to produce at which moment, and how much capacity it needs to keep in reserve. Additionally, the electricity producer needs aggregate usage data of the individual customers for billing purposes. Furthermore, the grid operator, i.e. the company that controls the infrastructure for the distribution and transportation of electricity, also needs accurate data about electricity flows and status information about essential grid components in order to optimise their networks.

Another important stakeholder is the actual consumer. We want the consumers to be informed and aware of their energy consumption and its costs with the hope that this might lead to energy savings.

The main question here is how much information must be revealed in order to achieve the needs of the electricity producer, grid operator and the consumer? We do not want to leave out more information than necessary to the different parties. As an example, some consumers might want to follow their energy consumption in order to lower their billing but they may not want their electricity producer to follow their habits and behaviours.

## Goal
The purpose of this project is to devise a system where smart meters could encrypt their generated readings and send it to the server/collector. The server should be able to sum up the encrypted readings for each client and after specified period of time, access the plaintext. The main part is that the server does not have access to the individual readings, only the aggregated result. In order to sum encrypted readings, homomorphic encryption was used.

## Application design
The system is divided into a client and server model. The server node, or the collector, is responsible for computing a global group key which is based on all the public keys generated by the clients. When the group key has been created the server will begin to distribute the key to all clients which will use this key to encrypt data. The server is also in charge of co-ordinating decryption of messages. In order to decrypt messages the server needs to collaborate with *all* clients to successfully decrypt a message. This is called threshold decryption and eliminates a central entity which holds the private key used for decryption. Instead, the system requires collaboration between nodes which improves the privacy for all the users.

The clients are responsible for generating *(fake)* energy readings every *n* seconds, then encrypt the readings with the group key computed by the server, and finally send the data to the server. In turn, the server will sum all encrypted readings for each client, and after a specified number of received readings per client the server will request decryption of aggregated result. As mentioned before, the clients needs to collaborate with the server in order to decrypt a message.

### How to run
* Run `bash install.sh` as root in order to install necessary dependencies.
* The `node_list.txt` file contains all the nodes participating in the system. This file should be the same for all nodes. The first entry in the file designates the server.
* For clients, you need to make sure the correct network interface is set in `src/client.py` line 20. As of now there is no function that auto-detects it.
* To run as a client: run.py --mode=client
* To run as a server: run.py --mode=server

### Thanks to
* [Petlib](https://github.com/gdanezis/petlib)
* [PET-Exercises](https://github.com/gdanezis/PET-Exercises)
