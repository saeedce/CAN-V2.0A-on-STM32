# CAN-V2.0A-on-STM32
Implements CAN 2.0A protocol on STM32F429 and STM32F103 for reliable data communication and intein embedded systems. Includes full CAN setup: filter configurations, message transmission,rrupt-driven reception. Ideal for automotive or industrial applications needing robust CAN communication.


CAN Protocol Implementation on STM32F429ZGT Microcontroller
This project demonstrates the implementation of the CAN 2.0A protocol on the STM32F429ZGT microcontroller, focusing on configuring parameters, defining scenarios, implementing communication, and addressing challenges associated with CAN protocol deployment.

Overview
The CAN (Controller Area Network) protocol is a robust communication standard commonly used in automotive and industrial automation to facilitate efficient data exchange between multiple nodes. This project specifically implements the CAN 2.0A standard on the STM32F429ZGT microcontroller, aiming to ensure reliable data transfer between nodes under predefined conditions.

Project Components
1. Tools and Equipment
STM32F429ZGT Microcontroller: A high-performance microcontroller featuring ARM Cortex-M4 architecture, selected for its robustness in handling communication protocols like CAN.
CAN Transceiver (ISO1050): Facilitates the transmission of CAN signals between the STM32F429 and other nodes, supporting differential signaling to minimize errors.
STM32F100 Microcontroller: Used as an additional node in the CAN network, programmed to transmit messages at a fixed interval and to toggle an LED on a specific GPIO pin upon receiving a CAN message.

2. Scenario Definition
In this implementation, two distinct modes of operation are defined:

Mode 0: The primary node periodically transmits data packets with a counter, simulating a continuous data transmission scenario.
Mode 1 (Echo Mode): Incoming CAN messages are echoed back to the source node, serving as a test configuration to validate message reception and transmission.
The STM32F100 node is equipped with an LED that toggles upon receiving a message, providing a visual indicator of communication status.

3. Implementation Method
CAN Initialization: Configures CAN1 on the STM32F429ZGT, setting the CAN filter, mode, and interrupt handlers.
Message Transmission: Utilizes a custom CAN_Send() function to send an 8-byte data frame containing predefined characters and a counter.
Message Reception: Configured with an interrupt handler to receive data on CAN_RX_FIFO0. On message reception, data is stored in the buffer and processed as per the defined mode.

4. Challenges and Solutions
This project encountered specific challenges:

Timing Issues: Ensuring synchronization between nodes was critical; an incorrect timing configuration led to transmission failures, later resolved by adjusting the CAN timing parameters.
Interrupt Handling: Configuring the interrupt callback for CAN2 proved challenging due to discrepancies in configuration settings, which required additional fine-tuning to achieve reliable message handling.

5. Results
The implementation achieved stable communication between nodes, with the STM32F429 node successfully handling both transmission and reception of messages as per the specified modes. The LED on the STM32F100 confirmed message reception, allowing for easier debugging and validation of the communication.

Usage
Clone the Repository: Clone this repository to obtain the source code and configurations.
Build and Flash the Code: Using an IDE like STM32CubeIDE, compile and flash the code onto the STM32F429ZGT.
Monitor Communication: Set up a CAN analyzer or a second STM32 device to observe the transmitted and received messages.

CAN Configuration
The CAN configuration on the STM32F429ZGT microcontroller has been set up to enable reliable CAN communication in a standard mode. The key configuration parameters are as follows:

CAN1 Settings:

Prescaler: Set to 16 to control the message transmission rate based on the system clock frequency.
CAN Mode: Configured to CAN_MODE_NORMAL for standard message transmission and reception.
Timing Segments:
Sync Jump Width (SyncJumpWidth): Set to one time quantum (CAN_SJW_1TQ) to synchronize bit timing.
Time Segments 1 and 2: Set TimeSeg1 to two time quanta (CAN_BS1_2TQ) and TimeSeg2 to one time quantum (CAN_BS2_1TQ), establishing a stable bit timing structure.
CAN Filter Configuration:

Filter Bank: Assigned to bank 0 for CAN1.
Filter Mode: Configured to CAN_FILTERMODE_IDMASK, allowing messages that match the filter’s address mask.
Filter FIFO Assignment: Messages are stored in CAN_RX_FIFO0.
Filter Identification: Set to 0, allowing the reception of all messages.
Filter Mask: Also set to 0, to receive all messages without restrictions.
Filter Scale: Configured to CAN_FILTERSCALE_32BIT for full address filtering.
Filter Activation: Enabled (ENABLE) to process incoming messages.
Interrupt Activation:

To ensure CAN messages are received in real-time, the receive message pending interrupt CAN_IT_RX_FIFO0_MSG_PENDING is enabled using HAL_CAN_ActivateNotification. This calls the HAL_CAN_RxFifo0MsgPendingCallback function upon message reception.
These configurations allow the STM32F429ZGT to transmit and receive CAN messages in standard conditions and efficiently handle message reception events.



License
This project is licensed under the MIT License, making it freely available for use, modification, and distribution.
