<script>
	let cvFile = null;
	let firstName = '';
	let mobile = '';
	let email = '';
  
	async function uploadCV() {
	  if (cvFile && firstName && mobile && email) {
		const base64Data = await convertToBase64(cvFile);
		const postData = {
		  cv: base64Data,
		  firstName: firstName,
		  mobile: mobile,
		  email: email
		};
  
		const response = await fetch('/api/upload', {
		  method: 'POST',
		  headers: {
			'Content-Type': 'application/json'
		  },
		  body: JSON.stringify(postData)
		});
  
		// Handle the response as needed
		console.log(response);
	  }
	}
  
	function convertToBase64(file) {
	  return new Promise((resolve, reject) => {
		const reader = new FileReader();
		reader.onload = () => resolve(reader.result.split(',')[1]);
		reader.onerror = error => reject(error);
		reader.readAsDataURL(file);
	  });
	}
  </script>
  
  <style>
	@import 'bootstrap/dist/css/bootstrap.min.css';
  </style>
  
  <main class="container">
	<h1>Details</h1>
	<input type="file" class="form-control-file" accept=".pdf,.doc,.docx" onchange="cvFile = event.target.files[0]" />
	<input type="text" class="form-control" placeholder="First Name" value="{firstName}" oninput="firstName = event.target.value" />
	<input type="text" class="form-control" placeholder="Mobile Number" value="{mobile}" oninput="mobile = event.target.value" />
	<input type="text" class="form-control" placeholder="Email" value="{email}" oninput="email = event.target.value" />
	<button class="btn btn-primary" onclick="uploadCV()">Submit</button>
  </main>
  