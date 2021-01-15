trigger UpdateAccountField on Opportunity (before insert) {

    List<Account> listAccount =new List<Account>();
        Set<Id> setids = new Set<Id>();
        Map<Id,Opportunity> mapAccIdOpp = new Map<Id,Opportunity>();
    
        if(Trigger.isInsert || Trigger.isUpdate && Trigger.isAfter)
        {
            for(Opportunity opp :Trigger.New)
            {
                setids.add(opp.AccountId);  }
                for(Opportunity opp :Trigger.New){
              mapAccIdOpp.put(opp.AccountId,opp);
               
            }
            for(Account opp : [select Id, is_gold__c from Account where Id IN :setids])
            {
              if(mapAccIdOpp.get(opp.Id).Amount>20000)
              {
                 opp.is_gold__c=True;
                 listAccount.add(opp);
              }
                
             else
             {
               opp.is_gold__c=False;
               listAccount.add(opp);
             }
             if(listAccount.size()>0)
             update listAccount;  
         }       
    }
}
