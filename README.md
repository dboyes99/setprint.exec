# SETPRINT EXEC
z/VM CMS tool for selecting print destinations by name.  

SETPRINT EXEC
 
Brief Summary of Statement:
 
 SETPRINT allows selecting which printer to use when generating CMS
 output.
 
Use the SETPRINT command to:
 
 SETPRINT allows selecting which printer to use when generating CMS
 output. Since actual line printers are somewhat scarce in the modern
 world, this tool assumes that printers are controlled by a virtual
 machine instead of CP, the printers are selected by a combination of
 tags in the file PRINTERS NAMES.

The format of the SETPRINT command is:
 
 >>--SETPRINT----DEST--destination-----------------------------------><
 
Operands
 
DEST
     indicates the user is selecting where printed output is to be
     directed for subsequent CMS PRINT commands.
 
destination
     selects the human-friendly name of an entry in PRINTERS NAMES that
     defines the combination of RSCS options used when output is
     generated.
 
How to Specify
 
 SETPRINT assumes that all printers on a system are driven by virtual
 machines, generally RSCS. The available printers on your system are
 defined by the file PRINTERS NAMES. The tool uses the first PRINTERS
 NAMES it finds in the CMS search order to look up a human-friendly
 name for a group of RSCS options and optional tags to use when
 printing a file. SETPRINT is intended to be called from the command
 line or from the user's PROFILE EXEC whenever a user wishes to
 designate a new device to print subsequent output, or (in the case of
 calling it in PROFILE EXEC), to set the initial default printer for
 that CMS session.
 
Usage Notes
 
   1.  Printers are defined to SETPRINT by means of the file PRINTERS
       NAMES on any accessed disk. The file is a NAMES format file as
       discussed in the CMS Commands and Utilities manual under the
       entry for NAMEFIND. Each publically available printer to be
       available from this system should have a entry in this file. An
       entry may look similar to the following:
 
         * :printer.<human friendly nickname>
         * :desc.<text description of printer>
         * :location.<physical location of printer>
         * :controller.<name of RSCS or other machine supervising printing>
         * :node.<RSCS node name owning printer>
         * :device.<RSCS link name for printer device>
         * :tags.<tag priority for printer jobs>
         * :defform.<default form for jobs>
         * :defclass.<default class of print job>
         * :defdist.<default distribution code for jobs>
         * :notes.<any special info about the printer, like extra charges for
         * use>
 
       The * in column 1 indicates a comment. When encountered, all
       text until the end of the current line is ignored.
 
 +----------------------------------+---------------------------------+
 |Tag                               |Use                              |
 +----------------------------------+---------------------------------+
 | :printer.                        |  Identifies a human-friendly    |
 |                                  | name for a printer. Each entry  |
 |                                  | must begin with this tag and    |
 |                                  | the definition extends to the   |
 |                                  | next :printer. tag.             |
 +----------------------------------+---------------------------------+
 | :desc.                           | A text description of this      |
 |                                  | printer, usually including the  |
 |                                  | make and model, and the         |
 |                                  | capabilities of the printer.    |
 |                                  | This description is displayed   |
 |                                  | at the terminal when the        |
 |                                  | printer is selected, so         |
 |                                  | descriptions should be limited  |
 |                                  | to 80 characters.               |
 +----------------------------------+---------------------------------+
 
 +----------------------------------+---------------------------------+
 |›|Tag›%                             |›|Use›%                            |
 +----------------------------------+---------------------------------+
 | :location.                       | Physical location of the        |
 |                                  | printer. This tag should        |
 |                                  | include enough information to   |
 |                                  | reasonably locate the printer.  |
 |                                  | This description is displayed   |
 |                                  | at the terminal when the        |
 |                                  | printer is selected, so         |
 |                                  | descriptions should be limited  |
 |                                  | to 80 characters.               |
 +----------------------------------+---------------------------------+
 | :controller.                     | The userid of the virtual       |
 |                                  | machine controlling the         |
 |                                  | printer. On modern z/VM         |
 |                                  | systems, this is generally RSCS |
 |                                  | but any virtual machine can be  |
 |                                  | designated here. Files will be  |
 |                                  | transferred to the virtual      |
 |                                  | reader of this user and         |
 |                                  | processed according to the tags |
 |                                  | interpreted by CP.              |
 +----------------------------------+---------------------------------+
 | :node.                           | If the printer is accessed via  |
 |                                  | RSCS, this tag indicates the    |
 |                                  | NJE node name of the system     |
 |                                  | controlling the printer. The    |
 |                                  | node must be accessible via NJE |
 |                                  | by this system.                 |
 |                                  |                                 |
 |                                  | If the printer is attached to   |
 |                                  | this system or driven by a RSCS |
 |                                  | LPR link, this tag should be    |
 |                                  | the NJE node name of your       |
 |                                  | system as returned by the CP    |
 |                                  | IDENTIFY command.               |
 +----------------------------------+---------------------------------+
 | :device.                         | If the printer is accessed via  |
 |                                  | RSCS, this tag indicates the    |
 |                                  | name of the printer link as     |
 |                                  | defined on the destination      |
 |                                  | system. If the printer is       |
 |                                  | defined on the local system,    |
 |                                  | this is the name of the RSCS    |
 |                                  | link for the 3270 printer or    |
 |                                  | LPR link as defined in RSCS     |
 |                                  | CONFIG.                         |
 +----------------------------------+---------------------------------+
 | :tags.                           | This tag indicates any          |
 |                                  | miscellaneous NJE tags that may |
 |                                  | apply to the device at the time |
 |                                  | output is actually committed to |
 |                                  | paper. The most common use is   |
 |                                  | to indicate the priority of the |
 |                                  | file (from 1 to 100), but any   |
 |                                  | valid NJE construct supported   |
 |                                  | by the CP spool commands can be |
 |                                  | placed here.                    |
 +----------------------------------+---------------------------------+
 | :defform.                        | This tag indicated the default  |
 |                                  | form to be requested for        |
 |                                  | printing at the printer site.   |
 |                                  | If this is a remote printer     |
 |                                  | accessed via NJE, this form     |
 |                                  | name must be valid on the       |
 |                                  | remote system.                  |
 +----------------------------------+---------------------------------+
 | :defclass.                       | This tag indicates a specific   |
 |                                  | output class is requested at    |
 |                                  | the site owning the printer.    |
 |                                  | This value must be valid at the |
 |                                  | remote site. If omitted, the    |
 |                                  | default class is determined by  |
 |                                  | the remote site, usually A.     |
 +----------------------------------+---------------------------------+
 | :defdist.                        | This tag indicates a defult     |
 |                                  | distribution code valid at the  |
 |                                  | site owning the printer. This   |
 |                                  | value must be valid at the      |
 |                                  | remote site. If omitted, the    |
 |                                  | default destination is          |
 |                                  | determined by the remote        |
 |                                  | system.                         |
 +----------------------------------+---------------------------------+
 | :notes.                          | This tag indicates additional   |
 |                                  | information that the user       |
 |                                  | should be aware of when         |
 |                                  | selecting this printer, such as |
 |                                  | attended hours or addional      |
 |                                  | costs associated with using     |
 |                                  | this printer. This entry is     |
 |                                  | displayed when the printer is   |
 |                                  | selected, so should be shorter  |
 |                                  | than the default line length    |
 |                                  | for the display.                |
 +----------------------------------+---------------------------------+
 
   2.  SETPRINT uses the first file named PRINTERS NAMES in the CMS
       search order to attempt to find the printer requested. If you
       wish to have printers defined that are not generally available
       to others on your system, you may create a private copy of
       PRINTERS NAMES on your own disks. Note that SETPRINT uses only
       printers contained in the first file located -- it does not fall
       through to a systemwide copy if a printer is not found in the
       first file located. If you use this capability, it is your
       responsibility to maintain entries for any systemwide printers
       you require.
 
   3.  Names used in the :printer. tag identifying the printer in
       PRINTERS NAMES can exceed 8 characters. The node and device tags
       must be valid NJE parameters (e.g. 1-8 chars each).
 
   4.  A device name of SYSTEM defers to the spooling system at the
       remote host to queue and print output.
 
   5.  LPR or 3270P links specified in PRINTERS NAMES must be defined
       to RSCS to operate properly.
 
Examples
 
   1.  To select printer YAYA listed in PRINTERS NAMES, specify:
 
         SETPRINT DEST YAYA
 
       If a user issues the above command, SETPRINT will produce output
       similar to:
 
         Output will be printed on:  YAYA
         Description:  HP Officejet 8600
         Location : Under the planter in Elena's office
         Ready;
 
   2.  A printer attached to remote system FOOVM as link PRT2 via RSCS,
       and which you want to refer to as a print destination of WIZBANG
       would be defined as follows. Users are alerted that supplying
       their own buckets is required.
 
         :printer.WIZBANG
           :desc.Water-Process 9000 Witch Melter
           :location.West Turret, Castle of the West, 9th floor, rm. 1168W
           :controller.RSCS
           :node.FOOVM
           :device.PRT2
           :notes.BYOB (bring your own bucket)
 
       To select this printer, specify SETPRINT DEST WIZBANG and grab
       an approved bucket.
 
   3.  If a user wishes to print to a printer controlled by RSCS node
       FOOBAR (your local system) using a LPR link named RANKIN in RSCS
       CONFIG, the entry in PRINTERS NAMES might appear as follows:
 
         :printer.LPRHERSHEY
           :desc.LPR accessible printer (Caramel Fudge 2000)
           :location.3D Printer Room, Candy Land, Hershey Park
           :controller.RSCS
           :node.FOOBAR
           :device.RANKIN
 
   4.  If a user wishes to print output on a JES-managed spooled
       printer on a remote z/OS system named CARDIFF using output class
       Q and form ORANGE, the entry might appear similar to this:
 
         :printer.Wales-Rabbit
           :desc.Wascally Wabbits from Wales Won't Win
           :location.Somewhere South of Sanity
           :node.CARDIFF
           :device.SYSTEM
           :defclass.Q
           :defform.ORANGE
