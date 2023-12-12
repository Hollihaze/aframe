
// Globe.js

import React, { useRef, useState } from 'react';
import { Canvas } from 'react-three-fiber';
import { Sphere } from '@react-three/drei';
import axios from 'axios';

const Globe = () => {
  const globeRef = useRef();
  const [markers, setMarkers] = useState([]);

  // Fetch markers data from a sample API
  const fetchMarkers = async () => {
    try {
      const response = await axios.get('https://api.example.com/markers');
      setMarkers(response.data);
    } catch (error) {
      console.error('Error fetching markers:', error);
    }
  };

  return (
    <Canvas>
      <ambientLight />
      <pointLight position={[10, 10, 10]} />
      <Sphere ref={globeRef} args={[5, 64, 64]}>
        <meshStandardMaterial
          map="/path/to/earthTexture.jpg" // Replace with the path to your Earth texture
        />
      </Sphere>

      {/* Render markers on the globe */}
      {markers.map((marker) => (
        <mesh key={marker.id} position={[marker.longitude, marker.latitude, 0]}>
          <sphereGeometry args={[0.1, 16, 16]} />
          <meshStandardMaterial color={0xff0000} />
        </mesh>
      ))}
    </Canvas>
  );
};

export default Globe;






// App.js

import React, { useState } from 'react';
import Globe from './Globe';

const App = () => {
  const [searchQuery, setSearchQuery] = useState('');
  const [filter, setFilter] = useState('');

  const handleSearch = (query) => {
    // Implement search logic here
    console.log(`Searching for: ${query}`);
  };

  const handleFilterChange = (newFilter) => {
    // Implement filter change logic here
    console.log(`Filter changed to: ${newFilter}`);
  };

  return (
    <div>
      <input
        type="text"
        value={searchQuery}
        onChange={(e) => setSearchQuery(e.target.value)}
        placeholder="Search the globe..."
      />
      <button onClick={() => handleSearch(searchQuery)}>Search</button>

      <select value={filter} onChange={(e) => handleFilterChange(e.target.value)}>
        <option value="">Select Filter</option>
        {/* Add your preset filter options here */}
      </select>

      <Globe />
    </div>
  );
};

export default App;

