global class WhatsappApex {
@AuraEnabled(cacheable=true)
    global static Boolean getContactId(string contactId){
        Contact con = [select id, Phone from Contact where id=:contactId];
        //string whatsappNo = '+91'+con.Phone;
        
        Http http = new Http();
        HttpRequest req =new HttpRequest();
        req.setEndpoint('https://graph.facebook.com/v15.0/107277368913355/messages');
        req.setHeader('Content-Type', 'application/json');
        string token ='###################################################################################################';
        req.setHeader('Authorization','Bearer '+token);
        req.setBody('{"messaging_product":"whatsapp", "to": "911234567890", "type":"template", "template":{"name":"hello_world", "language": {"code": "en_US"}}}');
        req.setMethod('POST');
        HttpResponse res = http.send(req);
        system.debug('status body is '+res.getBody());
        system.debug('status Code is'+res.getStatusCode());
        
        if(res.getStatusCode()==200){
            return true;
        }
        return false;
        
    }
}
