# Packets & Frames

Packets and frames are small pieces of data that, when forming together, make a larger piece of information or message. However, they are two different things in the OSI model. A frame is at layer 2 - the data link layer, meaning there is no such information as IP addresses. Think of this as putting an envelope within an envelope and sending it away. The first envelope will be the packet that you send, but once you open, the envelope within still exists and contains data, the frame.

This process is called encapsulation. At this stage, it is safe to assume that when we are talking about anything IP addresses, we are talking about packets. When the encapsulating information is stripped away, we are talking about the frame itself.

Packets are an efficient way of communicating data across networked devices. Because this data is exchanged in small pieces, there is less chance of bottlenecking occurring across a network than large messages being sent at once.

For example, when loading an image from a website, the image is not sent to your computer as a whole, but rather small pieces where it is reconstructed on your computer. Take the image below as an illustration of this process. The cat's picture is divided into three packets, where it is reconstructed when it reaches the computer to form the final image.

![d47215ad75f503af0b06dacca9ebace6.svg](../../_resources/d47215ad75f503af0b06dacca9ebace6.svg)

Packets have different structures that are dependent upon the type of packet that is being sent. As we'll come to discuss, networking is full of standards and protocols that act as a set of rules for how the packet is handled on a device. With billions of devices connected on the internet, things can quickly break down if there is no standardization.

A packet using a protocol will have a set of headers that contain additional pieces of information to the data that is being sent across a network. Some notable header include:

| **Header** | **Description** |
| --- | --- |
| Time to Live | This field sets an expiry timer for the packet to not clog up your network if it never manages to reach a host or escape! |
| Checksum | This field provides integrity checking for protocols such as <span style="color: inherit;">TCP</span>/IP. If any data is changed, this value will be different from what was expected and therefore corrupt. |
| Source Address | The IP address of the device that the packet is being sent **from** so that data knows where to **return to**. |
| Destination Address | The device's IP address the packet is being sent to so that data knows where to travel next. |