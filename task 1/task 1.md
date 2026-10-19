
Project Vision:

1.Ease of Use:
Designing a simple and user-friendly system that empowers employees to manage and organize their own vacation time and schedules without complexity.
2.Streamlining the HR Role:
Eliminating the old workflow that required manager approval followed by HR verification. The HR department has been removed from the routine approval chain, and their role is now strictly limited to entering and updating employee data within the system.
3.Approval Mechanism:
Establishing the immediate manager's approval as a direct and primary step to approve or reject a request), with an exception for certain high-level employees whose requests may be approved directly without requiring manager approval.

------------

Functional Requirements :
1-  Employee, Manager, and HR authentication via the company's Intranet portal (SSO).

2- Vacation request submission by employees and tracking of available leave balances.

3- Direct manager approval or rejection of vacation requests.

4- Automatic approval of vacation requests for high-level employees within their available balance.

5- Automatic deduction and updating of leave balance upon request approval.

6- Automated email notifications to employees regarding request status (approval/rejection with reasons and specified date ranges).

7- Integration and synchronization between the legacy HR system, the new Vacation Tracking System, and the database.

8- Logging all transactions, status changes, rejections, and system overrides in an Activity Log.

9- Manager and HR authority to execute discretionary overrides and grant exceptional leave (e.g., award leave outside standard balance).

10- Dedicated HR and Admin interface to query and review leave records for all employees. 

-----------

Non-Functional Requirements :

1- Simple, clear, and user-friendly interface design for all employee tiers

2- Protection of the activity log against unauthorized modifications or tampering.

---------------

Constraints:

1- The system must not require any manual approvals or physical signatures.

2- The system must provide a Web Service interface to query vacation information for all employees.

3- Strict adherence to the standard calendar to prevent errors.

4- Exclusive reliance on the company's SSO/CAS system for authentication and identity verification.
--------

Actor :

1- Employee: The primary user of the system who uses it to manage their leave and vacation time.
2- System Administrator: The person responsible for monitoring the system's status and ensuring it operates smoothly without any failures or downtime.
3- Manager: An employee with administrative rights to approve or reject employees' vacation requests, and who is permitted to grant discretionary/award leave outside the employee's standard balance within system limits.
4- HR Officer: The human resources representative responsible for monitoring employee vacations and viewing all leave records, with strict restrictions against deleting any system data.

-----------

Pseudocode :

START Manage_Vacation_Request (Request_ID, Manager_ID, New_Status)

    IF Current_User.Role NOT IN ['Manager', 'HR_Admin'] THEN
        RETURN False
    ENDIF

    Request = Get_Vacation_Request_By_ID(Request_ID)
    IF Request IS NULL THEN
        RETURN False
    ENDIF

    IF New_Status == "Approved" THEN
        
        Total_Days = Calculate_Days(Request.Start_Date, Request.End_Date)
        Balance = Get_User_Balance(Request.User_ID, Request.Leave_Type_ID)

        IF Balance.Remaining_Days >= Total_Days THEN
            Balance.Remaining_Days = Balance.Remaining_Days - Total_Days
            Request.Status = "Approved"
            
            Update_User_Balance(Balance)
            Update_Vacation_Request(Request)
            
            Create_Audit_Log(Manager_ID, Request.User_ID, "Approved Request #" + Request_ID)
        ELSE
            RETURN False
        ENDIF

    ELSE IF New_Status == "Rejected" THEN
        
        Request.Status = "Rejected"
        Update_Vacation_Request(Request)
        
        Create_Audit_Log(Manager_ID, Request.User_ID, "Rejected Request #" + Request_ID)

    ENDIF

    Send_Email_Notification(Request.User_ID, Request.Status)

END Manage_Vacation_Request

<img width="612" height="442" alt="model" src="https://github.com/user-attachments/assets/bf369913-fa4b-4ac8-a497-c1fc1dd17f68" />


<img width="520" height="1142" alt="flowchart" src="https://github.com/user-attachments/assets/bb1e2ee5-c048-45f8-8e84-53ba8094b361" />


<img width="782" height="713" alt="use case" src="https://github.com/user-attachments/assets/7f363c61-90bc-49e8-8351-8b9deb12140e" />


<img width="866" height="1146" alt="Sequence Diagram" src="https://github.com/user-attachments/assets/3e547e5f-4769-443f-b06e-a83962173e83" />

