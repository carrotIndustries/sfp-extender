SFP extender
============


![Two green PCBs on a grey metal surface, connected by a mini HDMI to 
HDMI cable. The top one is roughly the size of an SFP transceiver, has an edge connector on the right edge, a mini HDMI receptacle on the left edge and is labeled "SFP extender".
The bottom one is is 50×80mm and is populated with an HDMI receptacle labeled "NOT HDMI", a set of pin headers and an SFP receptacle without the cage.](media/sfp-extender.jpg)

For a future project, I need to conveniently probe an SFP transceiver 
while it's operating. To do so, the SFP needs to be outside of its cage 
with both sides accessible. Rather than having the SFP fixed to the 
host, such as in 
[this](https://www.multilaneinc.com/products/ml4066-sfp/) 
implementation, this extender uses a mini-HDMI to HDMI cable to extend 
all signals on the SFP connector. HDMI cables are especially 
well-suited for this application for multiple reasons:

 - Cheap and readily available cables and connectors
 - Shielded differential pairs capable of at least 1Gb/s
 - Just enough pins for low speed signals
 
Apparently, I'm not the first one to recognize the benefits of using 
HDMI connectors and cables for non-HDMI applications. So far, I'm aware of it being used in
[JTAG test 
equipment](https://www.keysight.com/us/en/product/N1125A/x1149-boundary-scan-analyzer.html) and for
[stacking switches](https://cathode.church/@s0/110384062337605415).

As usual, these boards are made with [Horizon EDA](https://horizon-eda.org/).

# SFP part

![3D rendering of an SFP-sized green PCBA with a mini-HDMI connector on 
the left side.](media/sfp-extender.png)

This part plugs into the SFP receptacle, so it has to comply to the 
[SFP specification](https://members.snia.org/document/dl/26184). This 
dictated the use of a mini-HDMI connector since a HDMI connector 
doesn't fit within the width of an SFP or would block adjacent ports if 
mounted outside of the SFP cage. I initially considered using 
a micro-HDMI connector, but was unable to fan out all pins while still 
meeting clearance and via diameter rules.

Install R1 to short VccT to VccR. This is intended to balance the 
current on the HDMI cable in case the module draws much more current on 
one Vcc than on the other. This is unlikely to be relevant for most 
transceivers as VccT and VccR usually connected internally anyways.

It's important that this board is 1mm in thickness to fit an SFP 
receptacle. The differential pairs are designed for JLCPCB's 7628 
stackup. I specified to chamfer the edges of the SFP edge connector 
since it didn't cost anything extra, but this resulted in the copper 
being pulled back a bit from the edge to meet their design rules, so 
manually sanding the edges or just leaving them as-is is probably the 
better option.

A 3D-printed shell is currently [work in progress](sfp-extender/mech).

## BOM
 - [TE 2013978-2](https://www.digikey.de/en/products/detail/te-connectivity-amp-connectors/2013978-2/4022373) mini-HDMI connector
 - 0 Ohm 0603 resistor (optional, see text)

# Receptacle part


![3D rendering of an 5×8cm green PCBA with a HDMI connector on 
the left side and an SFP cage on the right side. In between, there are 
0.1" headers.](media/sfp-extender-receptacle.png)

In addition to interfacing the HDMI connector to an SFP receptacle, 
this board fans out all low-speed signals and power supplies to 0.1" 
headers for convenient probing or interception. The two left columns 
are intended to be connected with jumpers to connect all required 
signals. One can remove the jumpers as needed to intercept signals or 
measure current consumption. The rightmost column can 
be connected to an oscilloscope or logic analyzer. If interception or 
current measurement isn't required, the signals can be connected 
permanently with 
zero-ohm resistors on the bottom side.

To get to the the normally-inaccessible bottom side of an operating transceiver, 
one can cut the board at the silkscreen line and omit the SFP cage.

Apart from access to the the high-speed RX/TX signals, this board 
should provide all that's needed for developing, debugging or reverse 
engineering SFP transceivers.

The differential pairs are designed for JLCPCB's 7628 
stackup.

## BOM
 - [Molex 2086581051](https://www.digikey.de/en/products/detail/molex/2086581051/10493706) HDMI connector
 - [Amphenol UE75-A20-3000T](https://www.digikey.de/en/products/detail/amphenol-cs-commercial-products/UE75-A20-3000T/1242769) SFP receptacle
 - [Amphenol U77-A1114-30L1](https://www.digikey.de/en/products/detail/amphenol-cs-commercial-products/U77-A1114-30L1/3465018) SFP cage
 - 0.1" pin headers
 - 0 Ohm 0603 resistors (optional, see text)

# License

CERN Open Hardware Licence v1.2
