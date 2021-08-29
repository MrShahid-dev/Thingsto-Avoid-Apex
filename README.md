# Thingsto-Avoid-Apex
Things to avoid in APEX

##1. Initialization of List
Common SOQL 
List<Account> listAcc = new List<Account>([SELECT Id, Name , Email FROM ACCOUNT]);
  
  We can avoid the explicit initialization to get rid of the shallow copy by having the logic like below.
  List<Account> listAcc = [SELECT Id, Name , Email FROM ACCOUNT];
  
  # 2. Fetch fields without mentioning them in SOQL 
  
  Example 1: 
  Account objAcc = [SELECT Id, Name FROM Account Limit 1];
  system.debug(objAcc.Id+ '------' + objAcc.Name);
  
  Example 2 : 
   Account objAcc = [SELECT Id, Name FROM Account Limit 1];
    system.debug(objAcc.Id+ '------' +objAcc.RecordTypeId+ '---------' +objAcc.Name);
  
  Example 3: //Below will featch Id , RecordType Id and AccountTypeId along with the fields mentioned.
  Contact objCon = [SELECT Name, Account.Name FROM Contact LIMIT 1];
  System.debug(objCon.AccountId);
  
  
  # 3. Get value of Formula Field without any DML / SOQL 
  
  Example ::
  Measurement__c ObjMe = new Measurement__c(Length__c = 100, Breadth__c = 50);
  insert ObjMe;
  
  ObjMe = [SELECT Id, Area__c FROM Measurement__c WHERE id=: ObjMe.id];
  System.debug('Calculated Area : '+objMe.Area__c);

  Best Example: 
   Measurement__c ObjMe = new Measurement__c(Length__c = 100, Breadth__c = 50);
  List<FormulaRecalcResult> = Formula.recalculateFormulas(new list<Measurement__c>{objMe});
   System.debug('Calculated Area : '+objMe.Area__c);
  
  
  Referance :: https://developer.salesforce.com/docs/atlas.ja-jp.apexcode.meta/apexcode/apex_class_System_Formula.htm 
  
  # 4 Replace .size() with .isEmpty() wherever applicable
  
  Example : 
  
  if(lstAcc.size() > 0){
    update lstAcc;
  }
  
  Alternate Example : 
  
   if(!lstAcc.isEmpty()){
    update lstAcc;
  }
  
  
