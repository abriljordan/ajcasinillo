---
title: Ruby Port Scanner
description: Ruby based port scanner with GUI using gtk
date: 2024-10-02 00:00:00+0000
categories:
    - Ruby
tags:
    - Ruby
    - gtk3
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

 A simple GUI-based port scanner using Ruby with GTK (GIMP Toolkit) and the gtk3 gem. Below is an example of how you can create a port scanner with a graphical interface in Ruby using GTK.

How It Works:

    1.  GTK Widgets: The interface consists of an entry box for the host, start port, and end port, a button to start the scan, and a text view to display the scan results.
    2.  Port Scanner Logic: The port scanner logic is the same as the console-based version. It attempts to connect to each port in the given range and outputs the result in the text view.
    3.  Text View Output: The add_to_textview function appends each result to the text view as the ports are scanned.

## Port Scanner with GTK GUI

```ruby
require 'gtk3'
require 'socket'

class PortScanner
  def initialize
    @window = Gtk::Window.new("Port Scanner")
    @window.set_default_size(300, 200)
    @window.set_border_width(10)

    vbox = Gtk::Box.new(:vertical, 5)

    @ip_entry = Gtk::Entry.new
    @ip_entry.placeholder_text = "Enter IP address"
    vbox.pack_start(@ip_entry, expand: false, fill: true, padding: 5)

    @port_entry = Gtk::Entry.new
    @port_entry.placeholder_text = "Enter port range (e.g., 1-1000)"
    vbox.pack_start(@port_entry, expand: false, fill: true, padding: 5)

    scan_button = Gtk::Button.new(label: "Scan")
    scan_button.signal_connect "clicked" do
      scan_ports
    end
    vbox.pack_start(scan_button, expand: false, fill: true, padding: 5)

    @result_text = Gtk::TextView.new
    @result_text.editable = false
    scroll = Gtk::ScrolledWindow.new
    scroll.add(@result_text)
    vbox.pack_start(scroll, expand: true, fill: true, padding: 5)

    @window.add(vbox)
  end

  def scan_ports
    ip = @ip_entry.text
    port_range = @port_entry.text.split('-').map(&:to_i)
    start_port, end_port = port_range[0], port_range[1]

    @result_text.buffer.text = "Scanning...\n"

    Thread.new do
      open_ports = []
      (start_port..end_port).each do |port|
        begin
          socket = TCPSocket.new(ip, port)
          open_ports << port
          socket.close
        rescue Errno::ECONNREFUSED, Errno::ETIMEDOUT
          # Port is closed or filtered
        end
      end

      Gtk.queue do
        if open_ports.empty?
          @result_text.buffer.text += "No open ports found.\n"
        else
          @result_text.buffer.text += "Open ports: #{open_ports.join(', ')}\n"
        end
      end
    end
  end

  def run
    @window.show_all
    Gtk.main
  end
end

scanner = PortScanner.new
scanner.run
```