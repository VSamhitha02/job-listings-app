# job-listings-app
Frontend 
job-listings/
├── components/
│   ├── JobCard.js
│   └── JobDetail.js
├── data/
│   └── jobsData.js
├── pages/
│   ├── index.js
│   ├── [id].js
├── styles/
│   └── globals.css
└── package.json
Step-by-Step Code Implementation

1. Create Job Data File
data/jobsData.js

javascript

const jobsData = [
  {
    id: 1,
    title: 'Frontend Developer',
    company: 'Company A',
    location: 'Remote',
    type: 'Full-Time',
    description: 'Looking for a skilled frontend developer to join our team.',
    fullDescription: 'You will be responsible for developing user interfaces...'
  },
  {
    id: 2,
    title: 'Backend Developer',
    company: 'Company B',
    location: 'New York',
    type: 'Part-Time',
    description: 'Seeking a backend developer experienced in Node.js.',
    fullDescription: 'Your main responsibilities will include developing APIs...'
  },
  {
    id: 3,
    title: 'Full Stack Developer',
    company: 'Company C',
    location: 'San Francisco',
    type: 'Full-Time',
    description: 'Looking for a full stack developer proficient in React and Node.',
    fullDescription: 'You will work on both frontend and backend...'
  },
  // Add more job listings as needed
];

export default jobsData;
2. Job Card Component
components/JobCard.js

javascript

import React from 'react';
import Link from 'next/link';

const JobCard = ({ job }) => {
  return (
    <div className="job-card">
      <h3>{job.title}</h3>
      <p>{job.company}</p>
      <p>{job.location}</p>
      <p>{job.type}</p>
      <p>{job.description}</p>
      <Link href={`/${job.id}`}>
        <button>View Details</button>
      </Link>
      <button onClick={() => alert(`Applying for ${job.title}`)}>Apply Now</button>
    </div>
  );
};

export default JobCard;
3. Job Details Component
components/JobDetail.js

javascript

import React from 'react';
import { useRouter } from 'next/router';
import jobsData from '../data/jobsData';

const JobDetail = () => {
  const router = useRouter();
  const { id } = router.query;
  const job = jobsData.find(job => job.id === parseInt(id));

  if (!job) return <p>Loading...</p>;

  return (
    <div>
      <h1>{job.title}</h1>
      <h2>{job.company}</h2>
      <p>{job.location}</p>
      <p>{job.type}</p>
      <p>{job.fullDescription}</p>
      <button onClick={() => alert(`Applying for ${job.title}`)}>Apply Now</button>
    </div>
  );
};

export default JobDetail;
4. Job Listings Page
pages/index.js

javascript
import React, { useState } from 'react';
import JobCard from '../components/JobCard';
import jobsData from '../data/jobsData';

const JobListings = () => {
  const [search, setSearch] = useState('');
  const [jobType, setJobType] = useState('All');
  const [currentPage, setCurrentPage] = useState(1);
  const jobsPerPage = 5;

  const filteredJobs = jobsData.filter(job => {
    const matchesSearch = job.title.toLowerCase().includes(search.toLowerCase());
    const matchesType = jobType === 'All' || job.type === jobType;
    return matchesSearch && matchesType;
  });

  const totalPages = Math.ceil(filteredJobs.length / jobsPerPage);
  const displayedJobs = filteredJobs.slice((currentPage - 1) * jobsPerPage, currentPage * jobsPerPage);

  return (
    <div>
      <h1>Job Listings</h1>
      <input
        type="text"
        placeholder="Search by Job Title"
        value={search}
        onChange={(e) => setSearch(e.target.value)}
      />
      <select onChange={(e) => setJobType(e.target.value)} value={jobType}>
        <option value="All">All Types</option>
        <option value="Full-Time">Full-Time</option>
        <option value="Part-Time">Part-Time</option>
        <option value="Remote">Remote</option>
      </select>
      <div className="job-listings">
        {displayedJobs.map(job => (
          <JobCard key={job.id} job={job} />
        ))}
      </div>
      <div className="pagination">
        <button onClick={() => setCurrentPage(prev => Math.max(prev - 1, 1))}>Previous</button>
        <span>Page {currentPage} of {totalPages}</span>
        <button onClick={() => setCurrentPage(prev => Math.min(prev + 1, totalPages))}>Next</button>
      </div>
    </div>
  );
};

export default JobListings;
5. Dynamic Routing for Job Details
pages/[id].js

javascript

import JobDetail from '../components/JobDetail';

const JobDetailPage = () => {
  return <JobDetail />;
};

export default JobDetailPage;
6. Global CSS Styling
styles/globals.css

css

body {
  font-family: Arial, sans-serif;
}

.job-listings {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.job-card {
  border: 1px solid #ccc;
  padding: 16px;
  width: 300px;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.pagination {
  margin-top: 20px;
}

.pagination button {
  margin: 0 5px;
}

h1, h2 {
  color: #333;
}

1. Add Hover Animation
To add a subtle hover animation to the job cards, you can update the CSS styles in your globals.css file.

Update styles/globals.css

css

.job-card {
  border: 1px solid #ccc;
  padding: 16px;
  width: 300px;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s; /* Add transition for animation */
}

.job-card:hover {
  transform: translateY(-5px); /* Hover effect */
}

.pagination {
  margin-top: 20px;
}

.pagination button {
  margin: 0 5px;
}

h1, h2 {
  color: #333;
}

/* Add this style for friendly message */
.no-jobs {
  color: red;
  font-size: 1.2em;
  margin-top: 20px;
}
2. Implement Error Handling
To display a friendly message when no job listings match the search or filter criteria, update the JobListings component in your pages/index.js file.

Update pages/index.js

javascript
Copy code
const JobListings = () => {
  const [search, setSearch] = useState('');
  const [jobType, setJobType] = useState('All');
  const [currentPage, setCurrentPage] = useState(1);
  const jobsPerPage = 5;

  const filteredJobs = jobsData.filter(job => {
    const matchesSearch = job.title.toLowerCase().includes(search.toLowerCase());
    const matchesType = jobType === 'All' || job.type === jobType;
    return matchesSearch && matchesType;
  });

  const totalPages = Math.ceil(filteredJobs.length / jobsPerPage);
  const displayedJobs = filteredJobs.slice((currentPage - 1) * jobsPerPage, currentPage * jobsPerPage);

  return (
    <div>
      <h1>Job Listings</h1>
      <input
        type="text"
        placeholder="Search by Job Title"
        value={search}
        onChange={(e) => setSearch(e.target.value)}
      />
      <select onChange={(e) => setJobType(e.target.value)} value={jobType}>
        <option value="All">All Types</option>
        <option value="Full-Time">Full-Time</option>
        <option value="Part-Time">Part-Time</option>
        <option value="Remote">Remote</option>
      </select>
      <div className="job-listings">
        {displayedJobs.length > 0 ? (
          displayedJobs.map(job => <JobCard key={job.id} job={job} />)
        ) : (
          <p className="no-jobs">No job listings match your search criteria.</p>
        )}
      </div>
      {displayedJobs.length > 0 && (
        <div className="pagination">
          <button onClick={() => setCurrentPage(prev => Math.max(prev - 1, 1))}>Previous</button>
          <span>Page {currentPage} of {totalPages}</span>
          <button onClick={() => setCurrentPage(prev => Math.min(prev + 1, totalPages))}>Next</button>
        </div>
      )}
    </div>
  );
};
