# DALL-E-Clone

DALL-E-Clone is a web application that uses OpenAI's DALL-E API to generate images from textual descriptions.

## Table of Contents

- [DALL-E-Clone](#dall-e-clone)
  - [Description](#description)
  - [Getting Started](#getting-started)
  - [Usage](#usage)
  - [Environment Variables and API Key Configuration](#environment-variables-and-api-key-configuration)
  - [Features](#features)
  - [Code Snippets](#code-snippets)
    - [DALL-E API Request](#dall-e-api-request)
    - [Image Generation Function](#image-generation-function)
    - [TailWindCSS Styling](#tailwindcss-styling)
  - [Tech Stack](#tech-stack)
  - [License](#license-mit-license)


## Description

DALL-E-Clone is a web application built using React.js that utilizes OpenAI's DALL-E API to generate images from textual descriptions. The application is designed to showcase the power of OpenAI's DALL-E API and how it can be used to create stunning images from textual inputs.

![Picture of Deployed App](./assets/dalle.png)

## Getting Started

To get started with DALL-E-Clone, simply visit the web application and enter a textual description. DALL-E-Clone will use the DALL-E API to generate an image that matches the description. No additional setup or installation is required.

[Deployed Site](https://dall-e2-e3hy.onrender.com/)

## Usage

Want to test on your local machine?

1. Clone the repository to your local machine

2. Install the necessary dependencies by running the following command in the root directory:

 `npm install`


3. Start the server by running the following command:

 `npm start`


4. To build the client for production, run the following command:

 `npm run build`


5. To run the client in development mode, run the following command:

`npm run dev`

### Environmental Variables and API Key Config
This project relies on certain environment variables and API keys to function properly. Follow the steps below to set them up:

#### Step 1: Create a `.env` file
Create a `.env` file in the root directory of the project. This file will store your environment variables.
#### Step 2: Obtain the OpenAI API Key
Visit the [OpenAI](https://openai.com/) website and sign up for an account if you don't have one already.
Once logged in, navigate to the [API keys](https://platform.openai.com/docs/api-reference) section of your account dashboard.

Click on the "Create API Key" button and copy the generated key.
#### Step 3: Add API Key and Other Variables to the `.env` File
```md
OPENAI_API_KEY=<your_openai_api_key>
MONGODB_URL=<your_mongodb_url>
```
Replace `<your_openai_api_key> `with the API key you obtained in Step 2, and `<your_mongodb_url>` with your MongoDB connection URL.
#### Step 4: (Optional) Obtain the Cloudinary API Key and Credentials
1. Visit the Cloudinary website and sign up for an account if you don't have one already.
2. Once logged in, navigate to the Dashboard and locate the "Account Details" section.
3. Copy the "API Key", "API Secret", and "Cloud name" from this section.
4. Add them to your `.env` file
```md
CLOUDINARY_CLOUD_NAME=<your_cloudinary_cloud_name>
CLOUDINARY_API_KEY=<your_cloudinary_api_key>
CLOUDINARY_API_SECRET=<your_cloudinary_api_secret>
```
#### Step 5: Load Enviornment Variables
Ensure that your application loads the environment variables from the `.env` file. You can use packages like `dotenv` to achieve this. In your project's entry file, add the following line:

`require('dotenv').config();`

After completing these steps, your project should be configured with the necessary environment variables and API keys. Remember to include the `.env` file in your `.gitignore` file to prevent it from being committed to your repository.
## Features

- Generates high-quality images from textual descriptions using OpenAI's DALL-E API
- Supports a wide range of textual inputs, from simple phrases to complex sentences and even paragraphs
- Offers a flexible and customizable interface that can be easily adapted to different use cases
- Provides an intuitive and user-friendly design for generating images from textual descriptions
- Uses Tailwind CSS for easy and responsive styling

## Code Snippets

### DALL-E API Request
When a client sends a request to this route with the image description (or prompt) in the request body, the server makes a call to the DALL-E API, receives the generated image in `base64` format, and sends it back to the client in the response.

```js
router.route('/').post(async (req,res) => {
    try{
        const { prompt } = req.body;

        const aiResponse = await openai.createImage({
            prompt,
            n: 1,
            size: '1024x1024',
            response_format: 'b64_json',
        });

        const image = aiResponse.data.data[0].b64_json;

        res.status(200).json({ photo: image });

    } catch (error) {
        console.log(error);
        res.status(500).send(error?.response.data.error.message)
    }
})
```

## Image Generation Function

The `generateImage` function is responsible for generating an image using DALL-E based on a provided textual prompt. 

```javascript
const generateImage = async () => {
  if (form.prompt) {
    try {
      setGeneratingImg(true);
      const response = await fetch(
        "https://dallecloneserver-pcz4.onrender.com/api/v1/dalle",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            prompt: form.prompt,
          }),
        }
      );

      const data = await response.json();
      setForm({ ...form, photo: `data:image/jpeg;base64,${data.photo}` });
    } catch (err) {
      alert(err);
    } finally {
      setGeneratingImg(false);
    }
  } else {
    alert("Please provide proper prompt");
  }
};
```

### TailWindCSS Styling
This snippet demonstrates how to style the image generated using Tailwind CSS.

```js
const Card = ({ _id, name, prompt, photo }) => (
  <div className="rounded-xl group relative shadow-card hover:shadow-cardhover card">
    <img
      className="w-full h-auto object-cover rounded-xl"
      src={photo}
      alt={prompt}
    />
    <div className="group-hover:flex flex-col max-h-[94.5%] hidden absolute bottom-0 left-0 right-0 bg-[#10131f] m-2 p-4 rounded-md">
      <p className="text-white text-sm overflow-y-auto prompt">{prompt}</p>

      <div className="mt-5 flex justify-between items-center gap-2">
        <div className="flex items-center gap-2">
          <div className="w-7 h-7 rounded-full object-cover bg-green-700 flex justify-center items-center text-white text-xs font-bold">
            {name[0]}
          </div>
          <p className="text-white text-sm">{name}</p>
        </div>
        <button
          type="button"
          onClick={() => downloadImage(_id, photo)}
          className="outline-none bg-transparent border-none"
        >
          <img
            src={download}
            alt="download"
            className="w-6 h-6 object-contain invert"
          />
        </button>
      </div>
    </div>
  </div>
);
```

## Tech Stack

This project is built using the following technologies:

- [MongoDB](https://docs.mongodb.com/): Used as the NoSQL database to store image and user-generated data.
- [Express.js](https://expressjs.com/): Provides the backend server and API endpoints for fetching and generating images.
- [React.js](https://reactjs.org/docs/getting-started.html): Utilized for building the frontend user interface and handling user interactions.
- [Node.js](https://nodejs.org/en/docs/): Serves as the runtime environment for the Express.js backend server.
- [OpenAI's DALL-E API](https://beta.openai.com/docs/api-reference/images/create): Integrated to generate images based on user prompts.
- [Tailwind CSS](https://tailwindcss.com/docs): Applied as the utility-first CSS framework for styling the frontend components.
- [Cloudinary](https://cloudinary.com/documentation): (Optional) Can be used for managing and serving the generated images.
- [Vite](https://vitejs.dev/guide/): Employs next-generation frontend tooling to enhance development experience and optimize production builds.



## License MIT License

Copyright (c) 2023 Jorge Zamora

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.





