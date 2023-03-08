import { LightningElement, api } from 'lwc';
import getContactId from '@salesforce/apex/WhatsappApex.getContactId';
export default class WhatsappIntegration extends LightningElement {
  
    @api recordId;

    handleClick(){
        getContactId({contactId:this.recordId})
        .then(result=>{
            window.alert("Message send Successfully");
        }).catch(error=>{
           window.alert("message not sent");
        })
    }
}
