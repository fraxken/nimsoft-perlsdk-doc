# Util API

### nimPostMessage(szSubject,uData,iLevel,szSup)

Post a message with a given subject to NimBUS. This is the generic method that is the basis for both nimAlarm
and nimQoS messages.

- szSubject is the post channel.
- udata     is a PDS record with user-data.
- iLevel    is the post priority (see Level constants).
- szSup     is the suppression definition.

```perl
my $PDS = Nimbus::PDS->new;
$PDS->number('level', 5);
$PDS->string('message', 'hello world!');

nimPostMessage('alarm2', $PDS->data);
```

> **Warning** This method only allow to post the field "**udata**".

### nimSendReply(hMsg, iRC, pds)

Reply to a triggered callback.

```perl
sub cb_execute {
    my ($hMsg) = @_;
    my $PDS = Nimbus::PDS->new(); 
    $PDS->string("status", "ok");

    nimSendReply($hMsg, NIME_OK, $PDS->data);
}
```

### nimSuppToStr(bHold,iNumber,iSeconds,szSuppKey) -> String

Create suppression definition string.

```perl
my $szSup = nimSuppToStr(0,0,0,"FileSystem|$name");
$szSup = nimSuppToStr(1, 0, 60, "");
```
Set new string variable.

### nimError2Txt(NIM_CODE) -> String

Translate the integer return code value into a human readable message!

```perl
my $rc = nimRequest(...);
print nimError2Txt($rc);
```

### nimInit(iFlag)

Initialize the NimBUS API. On windows that includes the WinSock initializing. This should be the first
NimBUS call in any application.

### nimEnd(iFlag)

Release all initialized NimBUS data. This should be the last NimBUS call in any application.

> nimInit and nimEnd are automatically triggered by Perl SDK.
