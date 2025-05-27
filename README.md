# CS-522-Assignment-Eight-Fragments-solved

Download Here: [CS 522 Assignment Eight—Fragments solved](https://jarviscodinghub.com/assignment/assignment-eight-fragments-solution/)

For Custom/Original Work email jarviscodinghub@gmail.com/whatsapp +1(541)423-7793

In this assignment, you will augment the UI of the Web-based chat app from the previous
assignment with a multi-pane interface. You will also get some practice with dialogs.
All of your projects for this submission should target Lollipop (Android 5.1).
Part 1: Multi-pane Navigation of Chat Rooms
Up until now, we have been assuming a single chat room that all chat participants share.
In this assignment, we will generalize this to allow multiple chat rooms. The Web chat
server from the previous assignment already supports multiple chat rooms, although you
only had to support communication through a “default” chat room. In this assignment,
we will extend this with the ability for the chat app to navigate between chat rooms using
a multi-pane UI. Ideally this would be used for a tablet interface, however it should also
work for a large cell phone or small tablet (e.g. the Nexus 7).
1. For a wide screen (at least 900dp), show a user interface that has a navigation pane
for chat rooms on the left (say 1/3 to ¼ of the screen), listing the chat rooms that are
available. Selecting one of these chat rooms from the list should open up, in the main
pane, a list of messages posted to that chat room (along with the identity of the
poster). Specify the fragments for the navigation pane (list of chatrooms) and for the
details pane (list of messages for a chatroom) in the layout for the activity (using the
fragment element1
). The chat room view should initially be empty. If the user
presses the BACK button while viewing the contents of a chat room, the chat room
view should resume an empty view, but the navigation view should remain.
2. Otherwise, show (just) a list of the chat rooms. Selecting one of these chat rooms
should replace the navigation pane with a details pane to display the messages in that
chat room. In other words, the behavior of your app in portrait orientation, when
viewing chat messages, is the same as it was in the previous assignment. Your app
should be able to dynamically switch between these two forms of user interfaces as
the orientation of the device changes. Android will perform this switching for you, as
long as you don’t prevent adaptation to orientation changes.
In a similar way, adapt the user interface for viewing other users with whom you are
exchanging messages. In landscape mode, show a list of the users in a navigation pane
on the left. Selecting a user should display a list of chat messages from that particular
user, in the details pane. In portrait mode, show these two lists (of peers and peerspecific messages) on separate screens, as in previous assignments.
The main change in the data model for your app is that you will need to add a table for
chat rooms to the messages content provider, with a one-to-many relationship from chat
rooms to messages:
1 Anecdotally the dynamic addition and replacement of fragments appears to behave better with respect to
changes like screen re-orientation, but we will work with static fragments for simplicity in this assignment.
CREATE TABLE Chatrooms (
_id INTEGER PRIMARY KEY,
name TEXT NOT NULL,
…
);
CREATE TABLE Messages (
…
sender_fk TEXT NOT NULL,
chatroom_fk TEXT NOT NULL,
FOREIGN KEY sender_fk REFERENCES Peers(name) ON DELETE CASCADE,
FOREIGN KEY chatroom_fk REFERENCES Chatrooms(name) ON DELETE CASCADE
);
CREATE INDEX MessagesSenderIndex ON Messages(sender_fk);
CREATE INDEX MessagesChatroomIndex ON Messages(chatroom_fk);
When displaying chat messages for a particular peer, you should show for each message
the chatroom in which that message was posted. You should use a join on the Messages
and Chatrooms tables to combine the text of each message and the chatroom where it
was posted. For example:
SELECT Chatrooms.name, Messages.messageText, Messages.whenPosted
FROM Chatrooms JOIN Messages
ON Chatrooms._id = Messages.chatroom_fk
WHERE Messages.sender_fk = …
As before, the names of tables and columns should be defined as string literals, stitched
into String-valued expressions that produce prepared SQL code. Input arguments (e.g.
chat room names or peer names) should be factored into separate selection argument
arrays, to prevent SQL injection attacks.
Part 2: Dialogs for dialogues
You should define several dialog user interfaces for your app.
The first dialog supports the creation of a chat room. The creation of a chat room should
be provided as a menu item from the action bar. Choosing this option should launch a
dialog that prompts the user to enter the name of the new chat room. Provide a button for
creating the chat room, and another button for canceling the dialog. If the user confirms
the creation of the chat room, then it should be added to the navigation pane. If you have
set things up right, this latter part should happen automatically as a result of a refresh of
the cursor for the list of chat rooms, by the loader manager and cursor loader. If the chat
room already exists, then display another dialog that informs the user of this fact, and
give them a button for dismissing this warning dialog. Do not use toasts for this
notification.
Another dialog should be used to post a message. Again, the command to post a message
should be a menu item available from the action bar. When selected, it should display a
dialog message that prompts the user for the text of the message. The user confirms the
message to be sent by pressing a SEND button on the dialog. You should also provide a
CANCEL button on the dialog. Note that the user can always cancel a dialog by pressing
the BACK button, but also providing a CANCEL button can be good human factors.
As demonstrated in class, define a dialog class for each one of these dialogs. Follow
these guidelines for each dialog class:
1. The class should inherit from DialogFragment.
2. The class should define a static method, called launch, that encapsulates the logic for
launching the dialog. This dialog should create the dialog fragment, pass it any
arguments from the client, and then use the show() method to launch the dialog.
3. Define an explicit interface that defines any functionality that the dialog fragment
requires from the parent activity. The fragment should bind to the activity with this
interface in the onAttach callback.
4. In general you can define the dialog user interface either in onCreateView or in
onCreateDialog. To get you used to the more flexible and general way of doing
things, you should always create your dialog UI in onCreateView. You may still
want to define some customization logic in onCreateDialog, e.g., ensuring that there
is no title bar for the dialog, but the UI should be defined in onCreateView.
5. The business logic for updating any databases or invoking background services
should be defined in the parent activity, invoked from the dialog through the parent
interface, or invoked from dialog callbacks that are defined in the parent activity.
Submitting Your Assignment
Once you have your code working, please follow these instructions for submitting your
assignment:
1. Create a zip archive file, named after you, containing a directory with your name. E.g.
if your name is Humphrey Bogart, then name the directory Humphrey_Bogart.
2. In that directory you should provide the Android Studio project for your Android app.
3. Also include in the directory a report of your submission. This report should be in
PDF format. Do not provide a Word document. The report should contain a
screenshot of the multi-pane interfaces for messages and for users, as well as a
screenshot for each of the dialogs you are asked to implement. Include a brief
description for each screen shot.
In addition, record short mpeg or Quicktime videos of a demonstration of your
assignment working. Your videos should demonstrate starting the app, showing a list of
chatrooms, navigating to the messages in a chatroom, and using a dialog to add a
message to the chatroom. You should demonstrate the app working both the single-pane
and dual-pane interfaces, using different devices or orientations (e.g. telephone vs tablet).
You do not need to demonstrate interaction with the server. Make sure that your name
appears at the beginning of the video. For example, put your name in the title of the app.
Do not provide private information such as your email or cwid in the video.
Your solution should be uploaded via the Canvas classroom. Your solution should
consist of a zip archive with one folder, identified by your name. Within that folder, you
should have a single Eclipse project, for the app you have built. You should also provide
a report in the root folder, called README.pdf, that contains a report on your solution,
as well as videos demonstrating the working of your assignment.
