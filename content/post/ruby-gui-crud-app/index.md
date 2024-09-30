---
title: Ruby GUI CRUD App
description: Ruby GUI CRUD App using gtk3
date: 2024-09-30 00:00:00+0000
categories:
    - Ruby
tags:
    - Ruby
    - gtk3

weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

## Ruby GTK (Ruby-GNOME2)

Description: GTK is a powerful cross-platform toolkit for creating graphical user interfaces. Ruby-GNOME2 (now called gtk3 for Ruby) is the Ruby binding for the GTK library.

Features:

    * Extensive widget support.
    * Cross-platform (Linux, macOS, and Windows).
    * Well-documented and widely used.

Installation

```bash
    gem install gtk3    
```
## Ruby GUI CRUD App using gtk3

![Ruby GUI CRUD App using gtk3](ruby-gui.png "Ruby GUI CRUD App using gtk3")

###  Install the necessary gems:
    gtk3: For building the GUI.
    sqlite3: For managing the SQLite database.

```bash
    gem install gtk3 sqlite3
```

### Build the Application
Below is a simple Ruby script that demonstrates how to create a basic CRUD system for managing records (e.g., user information) with a GUI interface.


```ruby
require 'gtk3'
require 'sqlite3'

# Create or open an SQLite3 database
db = SQLite3::Database.new "test.db"

# Create a simple table if it doesn't already exist
db.execute <<-SQL
  CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY,
    name TEXT,
    email TEXT
  );
SQL

# GTK Application
class CrudApp
  def initialize
    @builder = Gtk::Builder.new
    create_window
    setup_signals
    @db = SQLite3::Database.new "test.db"
    refresh_users_list
  end

  def create_window
    @window = Gtk::Window.new("Ruby SQLite CRUD App")
    @window.set_size_request(400, 300)
    @window.signal_connect("destroy") { Gtk.main_quit }

    # Create a vertical box layout
    vbox = Gtk::Box.new(:vertical, 10)
    vbox.margin = 20

    # Entry fields for name and email
    @name_entry = Gtk::Entry.new
    @name_entry.placeholder_text = "Enter name"
    vbox.pack_start(@name_entry, expand: false, fill: true, padding: 5)

    @email_entry = Gtk::Entry.new
    @email_entry.placeholder_text = "Enter email"
    vbox.pack_start(@email_entry, expand: false, fill: true, padding: 5)

    # Buttons for CRUD operations
    hbox = Gtk::Box.new(:horizontal, 10)
    
    add_button = Gtk::Button.new(label: "Add")
    add_button.signal_connect("clicked") { add_user }
    hbox.pack_start(add_button, expand: true, fill: true, padding: 5)
    
    update_button = Gtk::Button.new(label: "Update")
    update_button.signal_connect("clicked") { update_user }
    hbox.pack_start(update_button, expand: true, fill: true, padding: 5)
    
    delete_button = Gtk::Button.new(label: "Delete")
    delete_button.signal_connect("clicked") { delete_user }
    hbox.pack_start(delete_button, expand: true, fill: true, padding: 5)

    vbox.pack_start(hbox, expand: false, fill: true, padding: 5)

    # Listbox for displaying users
    @listbox = Gtk::ListBox.new
    vbox.pack_start(@listbox, expand: true, fill: true, padding: 5)

    # Add the layout to the window
    @window.add(vbox)
    @window.show_all
  end

  def setup_signals
    @window.signal_connect("destroy") { Gtk.main_quit }
  end

  def add_user
    name = @name_entry.text
    email = @email_entry.text

    if name.empty? || email.empty?
      show_message("Name and Email cannot be empty!")
      return
    end

    @db.execute("INSERT INTO users (name, email) VALUES (?, ?)", [name, email])
    refresh_users_list
    clear_entries
  end

  def refresh_users_list
  @listbox.each { |child| @listbox.remove(child) }  # Clear current list

  @db.execute("SELECT * FROM users") do |row|
    list_row = Gtk::ListBoxRow.new
    list_row.instance_variable_set(:@user_id, row[0])  # Save user id for update/delete actions
    label = Gtk::Label.new("#{row[1]} - #{row[2]}")
    list_row.add(label)
    @listbox.add(list_row)
  end

  @listbox.show_all
end

def update_user
  selected_row = @listbox.selected_row
  return unless selected_row

  user_id = selected_row.instance_variable_get(:@user_id)
  name = @name_entry.text
  email = @email_entry.text

  if name.empty? || email.empty?
    show_message("Name and Email cannot be empty!")
    return
  end

  @db.execute("UPDATE users SET name = ?, email = ? WHERE id = ?", [name, email, user_id])
  refresh_users_list
  clear_entries
end

def delete_user
  selected_row = @listbox.selected_row
  return unless selected_row

  user_id = selected_row.instance_variable_get(:@user_id)
  @db.execute("DELETE FROM users WHERE id = ?", [user_id])
  refresh_users_list
  clear_entries
end

#  instance_variable_set(:@user_id, row[0]): This method attaches the user_id to each Gtk::ListBoxRow as an instance variable. The variable @user_id will hold the ID from the database.
#  instance_variable_get(:@user_id): This method retrieves the stored user_id from the selected row when updating or deleting the user.

  def clear_entries
    @name_entry.text = ''
    @email_entry.text = ''
  end

  def show_message(message)
    dialog = Gtk::MessageDialog.new(
      parent: @window,
      flags: :destroy_with_parent,
      type: :info,
      buttons_type: :close,
      message: message
    )
    dialog.run
    dialog.destroy
  end
end

# Run the application
app = CrudApp.new
Gtk.main
```
