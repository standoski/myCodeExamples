# myCodeExamples
Useful small lines of code: usually HTML, CSS and JS.

From time to time I will upload various lines of code (especially HTML, CSS and JS) that I helped me in various projects.



===============================================================================

## Frontend form validation

If you have a form where your visitors can upload various files and wish to implement a validation form for those files, validation on client side (frontend) that doesn't allow users to go further right after they select the file, below is one simple example with few lines of code and NO JS framework required. 

_(NOTE: If someone wish to upload other types of files and have some minimum knowledge of programming can bypass frontend validation, so always have a backend (server side) validation form, but this one is for good faith users)_

**HTML form:**

`<form action="/upload/" id="upl_image" class="label_normal" enctype="multipart/form-data" method="post" accept-charset="utf-8">`

  `<input type="text" id="fname" class="form-control" name="image_name" title="Title length betwen 5-70 characters (letters (a-z), numbers, spaces, commas and dots)." pattern="[a-zA-Z0-9:,\.\- ]{5,70}" maxlength="70" required value="">`

  `<input type="file" id="uploadFile" class="form-control input-sm" name="file" accept=".jpg,.jpeg" required />`

`</form>`


**JS code (nice and simple code of plainJS):**

`<script>`  
		`const input = document.querySelector('#uploadFile');`  
		`input.onchange = function(e) {`  
			`if (((input.files[0].size/1024)/1024) > 4) {`  
				`alert("Please optimize your image to maximum 4MB!");`  
				`input.value = "";`  
				`//this code validates the file size, here the maximum file allowed is 4MB`  
			`}`  
			`else if ((/^[a-z0-9\s\,\.\-\_]{5,50}\.(?:jpg|jpeg)$/i.test(input.files[0].name)) == false) {`  
				`alert("Filename between 5-50 characters (letters (a-z), numbers, spaces, dots, '-' and '_').");`  
				`input.value = "";`  
				`//and this one use regex for allow only files that have common characters that defines the name of a file + the extension; this example is for image files`  
			`}`  
		`};`  
`</script>`  


Any feedback is welcomed :)

===============================================================================

## How i stopped SPAM via contact form in CodeIgniter


For 2-3 months I received daily 4 to 15 SPAM emails via contact form.  
  
One of my sites runs on CodeIgniter, but this doesn't matter, because basically is PHP :) and this solution can be adjusted on 
any PHP environment.
  
So, one day I read about the "honeypot" spam protection (NOTE: The Google captcha has NO effect in blocking bot spam) and I've adapted  
for my needs with a devil's end :)).  
  
Let's get to work:  

1. First in CodeIgniter (/application/controllers) we need to add in the contact controller file few lines of code:  
  
		$this->form_validation->set_rules('phone', $this->language->get_text('phone', 'global'), 'max_length[0]');
		if (!empty($this->input->post('phone'))) {
			$this->output->set_status_header(400); //this is a the diabolic thing, because when the condition is not accomplished, will return a 400 error, which is a COMPLETELY blank page, not even a line of code
			exit;
		}
  
  
  
2. Now in the VIEW file for contact will add:  
  
<input type="text" id="phone" name="phone" value="<?= set_value('phone'); ?>">

The above line of code MUST be inside the <form> </form> tags, the form that submit what REAL visitors wish to send you.  
  
  
  
3. The above line of code (from 2) now MUST be hidden from REAL VISITORS, and the best method is with CSS. In your CSS file just add:  
  
#phone{display:none;}



/* personal experience after the above implementation and why I recommend it 100%, at least at this time */  

1) Since this implementation (Google reCapctha for me was totally useless) I haven't received even 1 single SPAM through  
contact form (for about 2 months).

2) When Google reCapctha or any other captcha system exist, REAL VISITORS everytime needs to accomplish those annoying captchas, now  
there isn't any captcha :).  
  
3) If you wonder why is this woking (at least at this time) is because this bots are built to write something in every  
input field that is found in <form> </form> and probably recognize by the type and name attribute what should be written in those fields,
AND if there's a field like that one created by me named "phone" (you can set any name, doesn't matter :)) with the condition to be left
EMPTY, bots don't know that and always they write something in that field and the script returns 400 error. (usually any input field
is in <form> to be completed, is not like this one that is hide it from real users)


Conclusion: A pretty small code, cleaned my email for the incredibly annoying SPAM messages to all kind of weird things and visitors
don't need to do any captcha test.


IMPORTANT: This solution can be used in any <form>, because in the past this stupid bots also created accounts :)), yes must validate the  
email, but the account is still there even if the account will never be activated.
  
