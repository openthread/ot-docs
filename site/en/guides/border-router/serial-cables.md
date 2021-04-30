# Connecting a USB Serial Cable to the Border Router

<section class="wf-byline" itemscope itemtype="http://schema.org/Person">
  <div class="attempt-left">
    <figure>
      <img itemprop="image" src="/images/contributors/ot-byline-ti.jpg" alt="Texas Instruments">
    </figure>
  </div>
  <section class="wf-byline-meta">
    <div class="wf-byline-name">
      <strong>By</strong>
      <span itemprop="name">
        <a href="https://github.com/DuaneEllis-TI">
          <span itemprop="givenName">Duane</span>
          <span itemprop="familyName">Ellis</span>
        </a>
      </span>
    </div>
    <div class="wf-byline-desc">
        Contributor from Texas Instruments
    </div>
  </section>
</section>

Use a USB serial cable to connect to your Border Router serial port for
debugging purposes.

## BeagleBone Black

The BeagleBone Black (BBB) uses the following serial port settings:

*   Baudrate: 115200
*   Data Bits: 8
*   Parity: None
*   Stop Bits: 1

Recommended cables:

*   [Adafruit USB to TTL Serial Cable](https://www.adafruit.com/product/954)
*   [FTDI USB to TTL Serial 3.3V
    Cable](https://www.digikey.com/products/en?keywords=768-1015-ND)

### How to connect

For detailed information on connecting serial cables to the BBB, see the
[Beagleboard Wiki](https://elinux.org/Beagleboard:BeagleBone_Black_Serial). For
both cables, the black wire should be connected to Pin 1 (GND), which is
closest to the Ethernet port.

> Warning: To avoid hardware damage, always ensure cables are properly connected
before use.

<figure>
<a href="/guides/images/otbr-cables-adafruit.png"><img src="/guides/images/otbr-cables-adafruit.png" width="600" border="0" alt="BeagleBone Black Adafruit" /></a><figcaption>Adafruit serial cable</figcaption>
</figure>

<figure>
<a href="/guides/images/otbr-cables-ftdi.png"><img src="/guides/images/otbr-cables-ftdi.png" width="600" border="0" alt="BeagleBone Black FTDI" /></a><figcaption>FTDI serial cable</figcaption>
</figure>

## Raspberry Pi

The Raspberry Pi (RPi) uses the following serial port settings:

*   Baudrate: 115200
*   Data Bits: 8
*   Parity: None
*   Stop Bits: 1

Recommended cable: [Adafruit USB to TTL Serial
Cable](https://www.adafruit.com/product/954)

To learn how to connect and enable the serial console on your RPi, see
[Adafruit's Using a Console Cable guide](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-5-using-a-console-cable/overview).
