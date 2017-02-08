---
-api-id: T:Windows.ApplicationModel.Activation.ContactMessageActivatedEventArgs
-api-type: winrt class
---

<!-- Class syntax.
public class ContactMessageActivatedEventArgs : Windows.ApplicationModel.Activation.IActivatedEventArgs, Windows.ApplicationModel.Activation.IContactActivatedEventArgs, Windows.ApplicationModel.Activation.IContactMessageActivatedEventArgs
-->

# Windows.ApplicationModel.Activation.ContactMessageActivatedEventArgs

## -description
Provides data when an app is activated to send a message to a contact.



> **JavaScript**
> This type appears as [WebUIContactMessageActivatedEventArgs](../windows.ui.webui/webuicontactmessageactivatedeventargs.md).

## -remarks
Windows 8.1 allows users to message their contacts from the [Contact Card](../windows.applicationmodel.contacts/contactmanager_showcontactcard.md) or Windows Search experience. By implementing the contact message activation contract, Windows can launch your app to send messages for the user.

To receive message activations, your app must register for the "windows.contact" extension category in its manifest. Under this extension, you must include a "LaunchAction" element with the "Verb" attribute equal to "message." You can then specify the "ServiceId" element to specify the type of messaging you support. For example, if your app handles standard text messages, you can specify a "ServiceId" of "telephone." If your app handles messaging over a web based service, like Skype, you can specify the domain name of that service, for example "skype.com."

If multiple apps have registered for this contract, the user can choose one of them as their default for handling messages.



Here is an example for manifest registration:

```xml

<m2:Extension Category="windows.contact" xmlns:m2="http://schemas.microsoft.com/appx/2013/manifest">
  <m2:Contact>
    <m2:ContactLaunchActions>
      <m2:LaunchAction Verb="message" DesiredView="useLess">
        <m2:ServiceId>telephone</m2:ServiceId>
      </m2:LaunchAction>
      <m2:LaunchAction Verb="message" DesiredView="useLess">
        <m2:ServiceId>skype.com</m2:ServiceId>
      </m2:LaunchAction>
    </m2:ContactLaunchActions>
  </m2:Contact>
</m2:Extension>

```



After you register in your manifest, your app can be activated for the contact message contract. When your app is activated, you can use the event information to identify the message activation and extract the parameters that help you complete the messaging scenario for the user.

For info about how to handle app activation through contact actions, see [Quickstart: Handling contact actions ](http://msdn.microsoft.com/library/397d8b2a-6255-4f37-8556-449a3be2ef3f) and [Quickstart: Handling contact actions ](http://msdn.microsoft.com/library/61bacc8a-24c9-4b3d-b77b-e0822467700c).

## -examples
Here is an example of the code you need to handle contact message activations for PSTN numbers and Skype Ids:

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
    if (args.Kind == ActivationKind.Contact)
    {
        var contactArgs = args as IContactActivatedEventArgs;
        if (contactArgs.Verb == Windows.ApplicationModel.Contacts.ContactLaunchActionVerbs.Message)
        { 
            IContactMessageActivatedEventArgs messageArgs = contactArgs as IContactMessageActivatedEventArgs;

     //get contact display info
     var contactName = messageArgs.Contact.DisplayName;
            var contactThumbnail = messageArgs.Contact.Thumbnail;

            if (messageArgs.ServiceId == "telephone")
            {
                var phoneNumber = messageArgs.ServiceUserId;
                //add messaging logic for PSTN numbers
            }
            else if (messageArgs.ServiceId == "skype.com")
            {
                var userId = messageArgs.ServiceUserId;
                //add messaging logic for Skype Ids
            }
        }
                
    }
}

```

```cpp
void App::OnActivated(IActivatedEventArgs^ args)
{
 if (args->Kind == ActivationKind::Contact)
 {
  auto contactArgs = dynamic_cast<IContactActivatedEventArgs^>(args);
  if (contactArgs->Verb == Windows::ApplicationModel::Contacts::ContactLaunchActionVerbs::Message)
  {
   auto messageArgs = dynamic_cast<ContactMessageActivatedEventArgs^>(contactArgs);

       //get contact display info
auto contactName = messageArgs->Contact->DisplayName;
   auto contactThumbnail = messageArgs->Contact->Thumbnail;

   if (messageArgs->ServiceId == "telephone")
   {
    auto phoneNumber = messageArgs->ServiceUserId;
    //add messaging logic for PSTN numbers
   }
   else if (messageArgs->ServiceId == "skype.com")
   {
    auto userId = messageArgs->ServiceUserId;
    //add messaging logic for Skype Ids
   }
  }
 }
}

```



## -see-also
[IContactMessageActivatedEventArgs](icontactmessageactivatedeventargs.md), [IContactActivatedEventArgs](icontactactivatedeventargs.md), [IActivatedEventArgs](iactivatedeventargs.md), [Handling Contact Actions sample](http://go.microsoft.com/fwlink/p/?LinkID=320151)