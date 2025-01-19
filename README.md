Step 1: Set up Next.js and Sanity CMS
Run the command to create a new Next.js project:
bash
npx create-next-app my-nextjs-app
Install Sanity CLI globally:
bash
npm install -g @sanity/cli
Run the command to initialize a new Sanity project:
bash
sanity init
Step 2: Fetch and display data from Sanity to Next.js frontend
Set up API routes in the Next.js project to fetch data from Sanity:

JavaScript
// pages/api/fetchData.js
import sanityClient from '@sanity/client';

const client = sanityClient({
  projectId: 'your-project-id',
  dataset: 'your-dataset',
  useCdn: true,
});

export default async function handler(req, res) {
  const data = await client.fetch('*[_type == "yourDocumentType"]');
  res.status(200).json(data);
}
Inject sample data into Sanity using the API:

JavaScript
// scripts/injectData.js
import sanityClient from '@sanity/client';

const client = sanityClient({
  projectId: 'your-project-id',
  dataset: 'your-dataset',
  token: 'your-write-token', // Add your token here
  useCdn: false,
});

const doc = {
  _type: 'yourDocumentType',
  title: 'Sample Title',
  description: 'Sample Description',
};

client.create(doc).then((res) => {
  console.log(`Document created, ID: ${res._id}`);
});
Create components/pages in Next.js to display data from Sanity:

JavaScript
// pages/index.js
import { useEffect, useState } from 'react';

export default function Home() {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch('/api/fetchData')
      .then((res) => res.json())
      .then((data) => setData(data));
  }, []);

  return (
    <div>
      <h1>Sanity Data</h1>
      <ul>
        {data.map((item) => (
          <li key={item._id}>
            <h2>{item.title}</h2>
            <p>{item.description}</p>
          </li>
        ))}
      </ul>
    </div>
  );
}
Ensure the Sanity project is properly connected to the Next.js project, and test the setup by injecting data and verifying it shows up on the frontend.

