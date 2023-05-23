<script>
	import 'bootstrap/dist/css/bootstrap.min.css';
  
	let fullName = '';
	let email = '';
	let mobile = '';
	let file = null;
  
	async function handleSubmit() {
	  // Create a FormData object to send the file and other data
	  const formData = new FormData();
	  formData.append('fullName', fullName);
	  formData.append('email', email);
	  formData.append('mobile', mobile);
	  formData.append('file', file);
  
	  try {
		// Make a POST request to your API endpoint
		const response = await fetch('https://api.recruitly.io/api/cvsubmit/bytes?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77', {
		  method: 'POST',
		  body: formData
		});
  
		// Check if the request was successful
		if (response.ok) {
		  console.log('Details stored successfully!');
		  // Reset the form
		  fullName = '';
		  email = '';
		  mobile = '';
		  file = null;
		} else {
		  console.error('Error storing details:', response.statusText);
		}
	  } catch (error) {
		console.error('Error storing details:', error);
	  }
	}
  
	function handleFileChange(event) {
	  // Retrieve the selected file
	  file = event.target.files[0];
	}
  </script>
  
  <main class="container">
	<form on:submit|preventDefault={handleSubmit}>
	  <div class="mb-3">
		<label for="fullName" class="form-label">Full Name:</label>
		<input type="text" class="form-control" id="fullName" bind:value={fullName} />
	  </div>
	  <div class="mb-3">
		<label for="email" class="form-label">Email:</label>
		<input type="email" class="form-control" id="email" bind:value={email} />
	  </div>
	  <div class="mb-3">
		<label for="mobile" class="form-label">Mobile:</label>
		<input type="tel" class="form-control" id="mobile" bind:value={mobile} />
	  </div>
	  <div class="mb-3">
		<label for="file" class="form-label">Choose File:</label>
		<input type="file" class="form-control" id="file" on:change={handleFileChange} />
	  </div>
	  <button type="submit" class="btn btn-primary">Submit</button>
	</form>
  </main>
  