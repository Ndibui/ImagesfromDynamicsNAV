# Get Images from NAV via web service #


### FUNCTION NAME ###
```
GetProfilePicture
```

### PARAMETERS ###

```
Var	 Name	      DataType	 Subtype
*************************************
	 StaffNo	  Text			
```

### RETURN VALUE ###

```
Name  		ReturnType
**********************
BaseImage	Text
```


### VARIABLES ###

```
Name			DataType	Subtype
*******************************************
ToFile			Text		
IStream			InStream		
Bytes			DotNet		System.Array.'mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'	
Convert			DotNet		System.Convert.'mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'	
MemoryStream	DotNet		System.IO.MemoryStream.'mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'
```


### CAL CODE ###

```
**********************************************************
GetProfilePicture(StaffNo : Text) BaseImage : Text
**********************************************************

"Employee Card".RESET;
"Employee Card".SETRANGE("Employee Card"."No.", StaffNo);

IF "Employee Card".FIND('-') THEN BEGIN
IF "Employee Card".Picture.HASVALUE THEN BEGIN
  "Employee Card".CALCFIELDS(Picture);
  "Employee Card".Picture.CREATEINSTREAM(IStream);  
  MemoryStream := MemoryStream.MemoryStream();
  COPYSTREAM(MemoryStream,IStream);
  Bytes := MemoryStream.GetBuffer();
  BaseImage := Convert.ToBase64String(Bytes);
END;
END;
```

### ASPX CODE ###
```
<asp:Image id="ImgProfilePic" runat="server" />
```

### C# CODE ###
```
string ProfilePicBase64 = "";
ProfilePicBase64 = cSite.ObjNav.GetProfilePicture(StaffNo);
ImgProfilePic.ImageUrl = "data:image/png;base64," + ProfilePicBase64;
```



### Improvements ###
check if image exits in folder X, if not fetch image & save to folder X.


