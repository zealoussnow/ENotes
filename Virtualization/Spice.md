# Spice协议学习

## 1. Spice and spice-related components

### The basic building blocks of Spice are:

* Spice protocol

* Spice server

	Implemented in  libspice, a VDI pluggable library.
	On the side, the server communicates with the remote client using the Spice Protocol
	and on the other side, it interacts with the VDI host application(e.g QEMU)

	> VDI(Virtual Device Interface)

	> VDI defines a set of interfaces that provide a standard way to publish virtual devices
	(e.g display device, keyboard, mouse) and enables different Spice components to interact
	with those devices.

* Spice client

	Spice cross-platform(linux & windows) client is the interface for the end user.

### Features

* Multiple Channels

	1. Main - Control and configuration

	2. Display - Graphic commands, images and video streams

	3. Inputs - Keyboard and mouse inputs

	4. Cursor - Pointer device position and cursor shape

	5. Playback - audio received from the server to be played by the client

	6. Record - audio capture on the client side

* Image Compression

	1. Quic

	2. LZ

	3. GLZ

	Conceptually, synthetic images are better compressed with LZ/GLZ and real images
	are better with Quic.

* Video Compression

* Mouse Modes

	1. Server Mode

	2. Client Mode

* Other Features

	1. Multiple Monitors

	2. Bidirectional Audio

	3. Lip-sync

	4. Migration

	5. Pixmap caching

## 2. Installation

* Prerequisites

	1. qpixman

	2. qcairo

	3. celt

	4. ffmpeg

	5. log4cpp

#### Reference Website

- [Spice入门](http://blog.chinaunix.net/uid-24709751-id-3982082.html)

- [QXL驱动分析](http://blog.csdn.net/hbsong75/article/details/9952121)

- [KVM进阶](http://blog.csdn.net/zhangzxing/article/details/8609379)