# PendingIntent 
A **PendingIntent** is a token that you give to a foreign application (e.g. **NotificationManager**, **AlarmManager**, 
Home Screen **AppWidgetManager**, or other **3rd party applications**), which allows the foreign application to use your application's 
permissions to execute a predefined piece of code.

Please go to this page for more information [StackOverflow](https://stackoverflow.com/questions/2808796/what-is-an-android-pendingintent).

Difference between passing Intent and PendingIntent
> **If you give the foreign application an Intent, it will execute your Intent with its own permissions. But if you give the foreign application a PendingIntent, that application will execute your Intent using your application's permission.**

StackOverflow answer
- @Johnny_D: it means what it says, in general, you would want to create an explicit Intent whose component name is an absolute name that unambiguously refers to one of your own classes. Otherwise, the Intent might get sent to an another application, which may cause problems since that Intent will be running under your application's permission. – Lie Ryan 

- Isn't this "handover of permissions" really bad security-wise? I have an application A that I gave several permissions. Then I have application B which uses a lot less permissions. If application A can start something in application B via a PendingIntent (allowing all permissions from A to be on B temporarily), can't B do whatever it can behind the scenes with that permission? For instance, A might have the permission to read the user's contacts but B does not. If A sends a PendingIntent to B, B can then read the contacts and do something malicious (like send it to a server). – Tiago

- @Tiago: In your case, if a privileged Application A gives Application B a pending intent so that B can send it when it wants to read a contact data. It is the responsibility of A to ask the user which contact data the user wants to give to B, and only give B that data. Pending Intent is a privilege escalation mechanism, and just like any privilege escalation mechanism, with great power comes great responsibility. It is the responsibility of the user to decide whether application B is trustworthy for the contact data the user selected. – Lie Ryan

- @LieRyan "It is the responsibility of A to ask the user which contact data the user wants to give to B, and only give B that data.". I missed that part. That way it is slightly more secure then. I thought app B would be able to loop through all the contacts (with the permission from app A). Thanks for clarifying! – Tiago

- **So what's a pending intent?**
  1.It's a token that your app process will give to the location process, and the location process will use it to wake up your app when   an event of interest happens. So this basically means that your app in the background doesn't have to be always running.When something   of interest happens, we will wake you up. This saves a lot of battery.


# Why PendingIntent is required ? I was thinking like

1. Why the receiving application itself cannot create the Intent or Why we cannot use a simple Intent for the same purpose.
   E.g.Intent bluetoothIntent= new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE);

2. If I send bluetoothIntent to another application, which doesn't have permission android.permission.BLUETOOTH_ADMIN, that receiving      application cannot enable Bluetooth with startActivity(bluetoothIntent).

3. The limitation is overcome using PendingIntent. With PendingIntent the receiving application, needn't have 
   android.permission.BLUETOOTH_ADMIN for enabling Bluetooth.
   A beautiful ilstration is on the link. [PendingIntent](http://android-pending-intent.blogspot.com/). **LOOK**

4. For Service Foreground and Background interchanging. 
   
# Difference Between Intent and PendingIntent

TAXI ANALOGY

**Intent**

Intents are typically used for starting Services. For example:

> Intent intent = new Intent(CurrentClass.this, ServiceClass.class);
  startService(intent);
 
This is like when you call for a taxi:

> Myself = CurrentClass
 Taxi Driver = ServiceClass
 
**PendingIntent**

You will need to use something like this:

> Intent intent = new Intent(CurrentClass.this, ServiceClass.class);
  PendingIntent pi = PendingIntent.getService(parameter, parameter, intent, parameter);
  getDataFromThirdParty(parameter, parameter, pi, parameter);
  
Now this Third party will start the service acting on your behalf. A real life analogy is Uber or Lyft who are both taxi companies.

You send a request for a ride to Uber/Lyft. They will then go ahead and call one of their drivers on your behalf.

Therefore:

Uber/Lyft ------ ThirdParty which receives PendingIntent
Myself --------- Class calling PendingIntent
Taxi Driver ---- ServiceClass

