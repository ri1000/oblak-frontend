<script lang="ts">
    import axios from 'axios';
    let selectedFile: File | null = null;
  
    function handleFileUpload(event: Event) {
      const input = event.target as HTMLInputElement;
      const file = input.files?.[0];
      if (file) {
        selectedFile = file;
        handleClick();
      }
    }
    async function uploadFile() {
      if (selectedFile) {
        const formData = new FormData();
        formData.append('file', selectedFile);
        try {
          const response = await axios.post < string > ('https://europe-west8-cloudcomp-397620.cloudfunctions.net/oblak-fun', formData);
          console.log('File uploaded successfully:', response.data);
          return response.data;
        } catch (error) {
          console.error('Error uploading file:', error);
          throw new Error()
        }
      }
    }
    let promise = uploadFile();
  
    function handleClick() {
      promise = uploadFile();
    }
  
    function clearScreen() {
      location.reload();
    }
  </script>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }
  
    .container {
      text-align: center;
    }
  
    .spinner-container {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 110vh;
    }
  
    .spinner {
      display: inline-block;
      position: relative;
      width: 200px;
      height: 200px;
      margin-bottom: 20px;
    }
  
    .spinner:after {
      background-color: #0056b3;
      content: " ";
      display: block;
      border-radius: 50%;
      width: 0;
      height: 0;
      margin: 8px;
      box-sizing: border-box;
      border: 32px solid #fff;
      border-color: #fff transparent #fff transparent;
      animation: spinner 1.2s infinite;
    }
  
    h1 {
      margin-bottom: 20px;
    }
  
    input[type="file"] {
      margin-bottom: 20px;
    }
  
    h2 {
      margin-bottom: 10px;
    }
  
    button {
      padding: 10px 20px;
      background-color: #007bff;
      color: #fff;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
  
    button:hover {
      background-color: #0056b3;
    }
  
    a {
      text-decoration: none;
      color: #007bff;
      font-size: 18px;
      margin-top: 20px;
      display: block;
    }
  
    a:hover {
      text-decoration: underline;
    }
  
    @keyframes spinner {
      0% {
        transform: rotate(0);
        animation-timing-function: cubic-bezier(0.55, 0.055, 0.675, 0.19);
      }
  
      50% {
        transform: rotate(900deg);
        animation-timing-function: cubic-bezier(0.215, 0.61, 0.355, 1);
      }
  
      100% {
        transform: rotate(1800deg);
      }
    }
  </style>
  <body>
    <div class="container">
      <h1>Calculate standard deviation of an input file using a <span style="color: chartreuse;"> serverless function!</span></h1>
      <hr>
      <input class="file-input" type="file" accept=".txt" on:change="{handleFileUpload}" /> {#if selectedFile}
              {#await promise}
              
              <div class="spinner-container">
                  <div class="spinner"></div>
              </div>
              {:then response} 
                  
              <h2>Result of standard deviation: {response.result}</h2>
              <h2>Processing time: {response.timeTaken}</h2>
              <button type="submit" on:click="{clearScreen}">Clear</button>
              {:catch error}
                  
              <h2 style="color: brown;">Error processing file!</h2>
              {/await}
          {/if}
    
          
              <a href="/">Back</a>
          </div>
      </body>