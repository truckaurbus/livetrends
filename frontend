var Client = function(config) {

    var self = this;
    //
    // Config
    //
    config.host = 'URL';

    this.config = config;

    //
    // Socket and Dom actions
    //
    
    this.requestHistory = function(){
         self.socket.emit('request:history');
    }
    this.requestAdditionalInfo = function(id){
	self.socket.emit('request:additionalinfo', id);
    }
    
    this.getFragmentHTML = function(data){'OUTER FRAGMET');
    }
    this.progress = function(percent, $element) {
		var progressBarWidth = percent * $element.width() / 100;
		$element.find('div').animate({ width: progressBarWidth }, 500).html(percent + "%&nbsp;");
    }
    
    this.statFragment = function(data, rank){ 
        return('YOUR FRAGMENT');
	
    }
  
    this.roundNumber = function (num) {
	   return num.toString().indexOf(".") != -1 ? num.toFixed(2) : num;
    }
  
   this.alertDisconnection = function() {
       $('.connection-status')
              .removeClass('connected')
              .addClass('disconnected')
              .empty()
              .html('<i class="fa fa-exclamation-triangle" title="Check your Internet Connection"></i>'); 

    }

    //
    // Connection
    //
    this.listen = function() {
        self.socket = io.connect(self.config.host, {
            reconnect: true,
            transports: self.config.transports
        });
        
        self.socket.on('connect', function() {
           self.requestHistory();  
        });
		
	self.socket.on('connect_failed', function(){
           console.log('Starting to Connect (again)?');
           $('.not-loading').empty().html('Connection Failed!<br /><br /><p class="messed-up-bro">If you keep continuing seeing this message, your Internet Connection may be too slow or your browser is be too old.</p>') ;
		   
        });
		
	self.socket.on('load:history', function(data){
            $(self.getFragmentHTML(data)).hide().prependTo('#live-viewer-trends').slideDown("slow");
            
	    $('label[data-id="'+data.id+'"]').one("click",function(){
                self.requestAdditionalInfo(data.id);
                $(this).next().show();
            });
        });
		
	
	
		
       self.socket.on('get:viewers', function (data){
   
            if($('#live-viewer-trends li[data-id="'+data.id+'"]').length){
                $('span[data-id="'+data.id+'"]').attr({
                          'data-livestamp':data.time, 
                          'data-time':data.time
                });  
						  
                $('li[data-id="'+data.id+'"]').hide(1000, function() {
                    $(this).remove();
		            $(this).hide().prependTo('#live-viewer-trends').slideDown("slow");
         
		            $('.additional-info[data-id="'+data.id+'"]').hide()
		                             .html('<img class="load-info-anime" src="spinner.gif>');
         
		            $('label[data-id="'+data.id+'"]').one("click",function(){
			            self.requestAdditionalInfo(data.id);
			            $(this).next().show();
		            });
 
		        }); 
	    
		    }
            else {     
		        $(self.getFragmentHTML(data)).hide()
					        .prependTo('#live-viewer-trends')
						    .slideDown("slow");
              

		        $('label[data-id="'+data.id+'"]').one("click",function(){
			         self.requestAdditionalInfo(data.id);
			         $(this).next().show();
		        });
			
	        }  
	    });
		
		
		
		
	    self.socket.on('get:additionalinfo',function(data){

		    var rank = self.roundNumber(data.percentile);
		    $('.additional-info[data-id="'+data.id+'"]').empty().html(self.statFragment(data, rank));
			
		    self.progress(rank, $('#bar-graph-'+data.id));

	    });

        
    }
    //
    // Initialize
    //
    this.start = function() {
        var self = this;
        this.listen();
        return this;
    }

}
