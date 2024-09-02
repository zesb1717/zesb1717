import React, { useState } from 'react';
import { BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer } from 'recharts';
import { Calendar, UserCheck, Clock, AlertTriangle, Phone, Info, CheckCircle } from 'lucide-react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';

const getStatus = (scheduleProgress, onSiteProgress) => {
  if (onSiteProgress === 100) return 'Completed';
  if (onSiteProgress === 0) return 'Not Started';
  if (onSiteProgress < scheduleProgress) return 'Behind Schedule';
  if (onSiteProgress > scheduleProgress) return 'Ahead of Schedule';
  return 'On Schedule';
};

const projectData = [
  {
    id: 1,
    description: "Kerja-kerja menurap jalan serta kerja berkaitan di tanah perkuburan islam batu satu",
    contractor: "Zulhelmee Radzuaan (Pak Man)",
    contact: "010927 5938",
    startDate: "29.7.2024",
    endDate: "23.9.2024",
    duration: "8 minggu",
    scheduleProgress: 63,
    onSiteProgress: 0,
    status: "Not Started",
    remark: "Kerja Bagi, peruntukkan Parlimen. Siasatan bersama TNB bagi izin lalu pada 3.9.2024 - isu laluan memasuki lot 14015 dan Lot 1916 (Tanah Hak Milik)"
  },
  {
    id: 2,
    description: "Kerja-kerja baikpulih longkang dan naiktaraf gelanggang bola keranjang serta kerja berkaitan di tok kong cina bilut valley, Bilut Bentong",
    contractor: "CM Merly Ent.",
    contact: "017998 5597",
    startDate: "29.7.2024",
    endDate: "7.10.2024",
    duration: "10 minggu",
    scheduleProgress: 50,
    onSiteProgress: 50,
    status: "On Schedule",
    remark: "Kerja Bagi, peruntukkan Parlimen. Pertukaran Longkang 9'"
  },
  {
    id: 3,
    description: "Kerja-kerja menukar bumbung serta kerja berkaitan di SJKC Khai Mun",
    contractor: "Wahdah Kalam Ent.",
    contact: "012909 9463",
    startDate: "29.7.2024",
    endDate: "23.9.2024",
    duration: "8 minggu",
    scheduleProgress: 100,
    onSiteProgress: 100,
    status: "Completed",
    remark: "Kerja Bagi, peruntukkan Parlimen."
  },
  {
    id: 4,
    description: "Kerja-kerja menurap dataran kejat bagi kegunaan parkir serta kerja berkaitan di perkuburan cina lurah bilut, bentong",
    contractor: "KYS Maju Ent",
    contact: "010986 1544",
    startDate: "29.7.2024",
    endDate: "23.9.2024",
    duration: "8 minggu",
    scheduleProgress: 63,
    onSiteProgress: 90,
    status: "Ahead of Schedule",
    remark: "Kerja Bagi, peruntukkan Parlimen. Coring Test 3.9.2024"
  },
  {
    id: 5,
    description: "Menaiktaraf dan membaikpulih dewan terbuka Kg.Kuala Repas, Bentong serta kerja-kerja berkaitan",
    contractor: "ELL Maria Ent",
    contact: "017961 1816",
    startDate: "22.7.2024",
    endDate: "16.9.2024",
    duration: "8 minggu",
    scheduleProgress: 75,
    onSiteProgress: 80,
    status: "Ahead of Schedule",
    remark: "Kerja Bagi, peruntukkan Dun."
  },
  {
    id: 6,
    description: "Menaiktaraf dan baikpulih balai raya RTP Lebu serta kerja-kerja berkaitan.",
    contractor: "Prisma",
    contact: "N/A",
    startDate: "TBD",
    endDate: "TBD",
    duration: "N/A",
    scheduleProgress: 0,
    onSiteProgress: 0,
    status: "KIV",
    remark: "Kerja-kerja dalam perancangan"
  },
  {
    id: 7,
    description: "Menaiktaraf dan membaikpulih balairaya chamang serta kerja-kerja berkaitan",
    contractor: "Prisma",
    contact: "N/A",
    startDate: "TBD",
    endDate: "TBD",
    duration: "N/A",
    scheduleProgress: 0,
    onSiteProgress: 0,
    status: "KIV",
    remark: "Kerja-kerja dalam perancangan"
  },
  {
    id: 8,
    description: "Kerja-kerja penyediaan tapak pembinan rumah baru mangsa banjir serta kerja-kerja berkaitan di Kampung Baru Sungai Perdak",
    contractor: "MDZ Jaya Ent",
    contact: "019923 7923",
    startDate: "7.8.2024",
    endDate: "4.9.2024",
    duration: "4 Minggu",
    scheduleProgress: 100,
    onSiteProgress: 100,
    status: "Completed",
    remark: "Proses dalam Pembinaan"
  },
  {
    id: 9,
    description: "Kerja-Kerja Membaiki Tandas serta kerja-kerja berkaitan di Masjid Chamang Batu Satu Jalan Tras",
    contractor: "SR REZQY TRADING",
    contact: "019942 2840",
    startDate: "29.7.2024",
    endDate: "21.10.2024",
    duration: "12 Minggu",
    scheduleProgress: 42,
    onSiteProgress: 42,
    status: "On Schedule",
    remark: "Projek telah siap sepenuhnya dalam proses pembayaran"
  },
];

const statusColors = {
  'Completed': 'bg-green-500',
  'Not Started': 'bg-gray-500',
  'Behind Schedule': 'bg-red-500',
  'Ahead of Schedule': 'bg-blue-500',
  'On Schedule': 'bg-yellow-500',
  'KIV': 'bg-purple-500'
};

const ProjectCard = ({ project }) => (
  <Card className="mb-4">
    <CardHeader>
      <CardTitle className="text-lg font-bold">Project {project.id}: {project.description}</CardTitle>
    </CardHeader>
    <CardContent>
      <div className="grid grid-cols-2 gap-2">
        <div className="flex items-center"><UserCheck className="mr-2" size={16} /> {project.contractor}</div>
        <div className="flex items-center"><Phone className="mr-2" size={16} /> {project.contact}</div>
        <div className="flex items-center"><Calendar className="mr-2" size={16} /> {project.startDate} - {project.endDate}</div>
        <div className="flex items-center"><Clock className="mr-2" size={16} /> {project.duration}</div>
        <div className="flex items-center col-span-2">
          <CheckCircle className="mr-2" size={16} />
          <span className={`px-2 py-1 rounded-full text-white ${statusColors[project.status]}`}>
            {project.status}
          </span>
        </div>
      </div>
      {project.status !== 'KIV' && (
        <>
          <div className="mt-4">
            <div className="flex justify-between mb-1">
              <span>Schedule Progress</span>
              <span>{project.scheduleProgress}%</span>
            </div>
            <div className="w-full bg-gray-200 rounded-full h-2.5">
              <div className="bg-blue-600 h-2.5 rounded-full" style={{width: `${project.scheduleProgress}%`}}></div>
            </div>
          </div>
          <div className="mt-2">
            <div className="flex justify-between mb-1">
              <span>On-Site Progress</span>
              <span>{project.onSiteProgress}%</span>
            </div>
            <div className="w-full bg-gray-200 rounded-full h-2.5">
              <div className="bg-green-600 h-2.5 rounded-full" style={{width: `${project.onSiteProgress}%`}}></div>
            </div>
          </div>
        </>
      )}
      {project.remark && (
        <div className="mt-4 flex items-start">
          <Info className="mr-2 text-blue-500 flex-shrink-0" size={16} />
          <p className="text-sm">{project.remark}</p>
        </div>
      )}
    </CardContent>
  </Card>
);

const ProgressChart = ({ data }) => (
  <ResponsiveContainer width="100%" height={300}>
    <BarChart data={data.filter(project => project.status !== 'KIV')}>
      <CartesianGrid strokeDasharray="3 3" />
      <XAxis dataKey="id" />
      <YAxis />
      <Tooltip />
      <Legend />
      <Bar dataKey="scheduleProgress" name="Schedule Progress" fill="#3b82f6" />
      <Bar dataKey="onSiteProgress" name="On-Site Progress" fill="#22c55e" />
    </BarChart>
  </ResponsiveContainer>
);

const Dashboard = () => {
  const [selectedProject, setSelectedProject] = useState(null);
  const [filter, setFilter] = useState('all');

  const filteredProjects = filter === 'all' 
    ? projectData 
    : projectData.filter(project => project.status.toLowerCase() === filter);

  return (
    <div className="container mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6">Project Dashboard</h1>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div>
          <h2 className="text-xl font-semibold mb-4">Project Progress Overview</h2>
          <ProgressChart data={projectData} />
        </div>
        <div>
          <h2 className="text-xl font-semibold mb-4">Project Details</h2>
          <select 
            className="w-full p-2 mb-4 border rounded"
            onChange={(e) => setSelectedProject(projectData.find(p => p.id === parseInt(e.target.value)))}
            defaultValue=""
          >
            <option value="" disabled>Select a project</option>
            {projectData.map(project => (
              <option key={project.id} value={project.id}>Project {project.id}: {project.description}</option>
            ))}
          </select>
          {selectedProject && <ProjectCard project={selectedProject} />}
        </div>
      </div>
      <div className="mt-8">
        <h2 className="text-xl font-semibold mb-4">All Projects</h2>
        <div className="mb-4">
          <label htmlFor="filter" className="mr-2">Filter projects:</label>
          <select 
            id="filter"
            className="p-2 border rounded"
            onChange={(e) => setFilter(e.target.value)}
            value={filter}
          >
            <option value="all">All Projects</option>
            <option value="kiv">KIV</option>
            <option value="not started">Not Started</option>
            <option value="on schedule">On Schedule</option>
            <option value="behind schedule">Behind Schedule</option>
            <option value="ahead of schedule">Ahead of Schedule</option>
            <option value="completed">Completed</option>
          </select>
        </div>
        {filteredProjects.map(project => (
          <ProjectCard key={project.id} project={project} />
        ))}
      </div>
    </div>
  );
};

export default Dashboard;


- ⚡ Fun fact: ...

<!---
zesb1717/zesb1717 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
