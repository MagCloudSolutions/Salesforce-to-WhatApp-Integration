import {api,track,LightningElement} from 'lwc';
import LightningModal from 'lightning/modal';
import getChatGPTResult from '@salesforce/apex/ChatGPT.getChatGPTResult';
export default class ChatGptIntegration extends LightningModal {
    @api content;
    @track searchTerm='';
    @track
    showSpinner=false;
    @track
    searchResultChatGPT=[];
    
    
    handleEnterKey(event){
           if(event.keyCode===13){
            this.searchTerm=event.target.value;
            this.showSpinner=true;
            getChatGPTResult({searchString:this.searchTerm}).then(result=>{
                let response = JSON.parse(JSON.stringify(JSON.parse(result)));
                if(response.error) {
                    this.searchResultChatGPT = response.error.message;
                  }else if (response.choices[0].text) {
                    this.searchResultChatGPT = response.choices[0].text;
                    this.searchResultChatGPT = this.searchResultChatGPT.replace(/\n/g, "<br />");
                    let tempScriptData = ''
                    tempScriptData = (response.choices[0].text.includes('<script>')) ? 'JS File: ' + response.choices[0].text.split('<script>')[1] : '';
                    tempScriptData = this.responseTextLWCJS.replace(/\n/g, "<br />");
 
                    this.searchResultChatGPT = this.responseData + this.responseTextLWCJS;
                    this.searchResultChatGPT = (this.searchResultChatGPT.includes('XML File:')) ? this.searchResultChatGPT.split('XML File:')[0] : this.searchResultChatGPT;
 
                    this.searchResultChatGPT.trim();
                  }
                  console.log('ss',JSON.stringify(this.searchResultChatGPT));
            }).catch(error=>{
                this.showSpinner = false
                console.log('error is '+error);
            })

        }
        
    }
    handleOkay() {
        this.close(this.content);
    }

}
