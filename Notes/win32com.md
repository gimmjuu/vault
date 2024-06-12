---
tags:
  - PYTHON
  - PKG
---
# `win32com`
## 📌 예제
```python
# Import our libraries
import win32com.client as win32
import pythoncom

# Define our Application Events
class ApplicationEvents:
    # Define an event inside of our application
    def OnSheetActivate(self, *args):
        print("Activated new sheet")

# Define our Workbook Events
class WorkbookEvents:
    # Define an event inside of our Workbook
    def OnSheetSelectionActivate(self, *args):
        # Print the arguments
        print(args)
        print(args[1].Address)
        args[0].Range("A1").Value = f"You selected cell {args[1].Address}"
        print("Activated new sheet")

# Get the active instance of Excel
xl = win32.GetActiveObject("Excel.Application")
# Assign our event to the Excel Application Object
xl_events = win32.WithEvents(xl, ApplicationEvents)

# Grab the workbook
xl_workbook = xl.Workbooks("Book1")
# Assign events to workbook
xl_workbook_events = win32.WithEvents(xl_workbook, WorkbookEvents)
# While there are messages keep displaying them
while True:
    # Display the message
    pythoncom.PumpWaitingMessages()
```
