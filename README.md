# Rentify
Frontend: React, HTML, CSS
Frontend Setup
# Initialize the Project:
npx create-react-app rentify-frontend
cd rentify-frontend
npm install axios react-router-dom
# Project Structure:
css
rentify-frontend/
├── public/
├── src/
│   ├── components/
│   │   ├── Auth/
│   │   │   ├── Login.js
│   │   │   └── Register.js
│   │   ├── Layout/
│   │   │   ├── Header.js
│   │   │   └── Footer.js
│   │   ├── Properties/
│   │   │   ├── PropertyList.js
│   │   │   ├── PropertyDetails.js
│   │   │   ├── AddProperty.js
│   │   │   └── EditProperty.js
│   │   ├── Dashboard.js
│   │   ├── Home.js
│   │   └── Profile.js
│   ├── api/
│   │   └── api.js
│   ├── App.css
│   ├── App.js
│   ├── index.js
│   └── setupProxy.js
├── .env
└── package.json
# Environment Variables:
REACT_APP_API_URL=http://rentifysever:5000/api

# API Setup
import axios from 'axios';

const API = axios.create({ baseURL: process.env.REACT_APP_API_URL });

API.interceptors.request.use((req) => {
    if (localStorage.getItem('token')) {
        req.headers.Authorization = `Bearer ${localStorage.getItem('token')}`;
    }
    return req;
});

export const register = (formData) => API.post('/users/register', formData);
export const login = (formData) => API.post('/users/login', formData);
export const getProfile = () => API.get('/users/profile');

export const createProperty = (formData) => API.post('/properties', formData);
export const getProperties = () => API.get('/properties');
export const getProperty = (id) => API.get(`/properties/${id}`);
export const updateProperty = (id, formData) => API.put(`/properties/${id}`, formData);
export const deleteProperty = (id) => API.delete(`/properties/${id}`);
export const likeProperty = (id) => API.post(`/properties/${id}/like`);
# Setup Proxy:
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function (app) {
    app.use(
        '/api',
        createProxyMiddleware({
            target: 'http://localhost:5000',
            changeOrigin: true,
        })
    );
};
# App Component:
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Home from './components/Home';
import Login from './components/Auth/Login';
import Register from './components/Auth/Register';
import Dashboard from './components/Dashboard';
import Profile from './components/Profile';
import PropertyList from './components/Properties/PropertyList';
import PropertyDetails from './components/Properties/PropertyDetails';
import AddProperty from './components/Properties/AddProperty';
import EditProperty from './components/Properties/EditProperty';
import Header from './components/Layout/Header';
import Footer from './components/Layout/Footer';

function App() {
    return (
        <Router>
            <Header />
            <Switch>
                <Route path="/" component={Home} exact />
                <Route path="/login" component={Login} />
                <Route path="/register" component={Register} />
                <Route path="/dashboard" component={Dashboard} />
                <Route path="/profile" component={Profile} />
                <Route path="/properties" component={PropertyList} exact />
                <Route path="/properties/:id" component={PropertyDetails} />
                <Route path="/add-property" component={AddProperty} />
                <Route path="/edit-property/:id" component={EditProperty} />
            </Switch>
            <Footer />
        </Router>
    );
}

export default App;
# Home Component:
import React from 'react';

function Home() {
    return (
        <div>
            <h1>Welcome to Rentify</h1>
            <p>Your one-stop solution for finding rental properties.</p>
        </div>
    );
}

export default Home;
# Header Component:
import React from 'react';
import { Link } from 'react-router-dom';

function Header() {
    return (
        <header>
            <nav>
                <ul>
                    <li><Link to="/">Home</Link></li>
                    <li><Link to="/properties">Properties</Link></li>
                    <li><Link to="/add-property">Add Property</Link></li>
                    <li><Link to="/profile">Profile</Link></li>
                    <li><Link to="/login">Login</Link></li>
                    <li><Link to="/register">Register</Link></li>
                </ul>
            </nav>
        </header>
    );
}

export default Header;
# Footer Component:
import React from 'react';

function Footer() {
    return (
        <footer>
            <p>&copy; 2024 Rentify. All rights reserved.</p>
        </footer>
    );
}

export default Footer;
# Login Component:
import React, { useState } from 'react';
import { useHistory } from 'react-router-dom';
import { login } from '../../api/api';

function Login() {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const history = useHistory();

    const handleSubmit = async (e) => {
        e.preventDefault();
        try {
                            const { data } = await login({ email, password });
                localStorage.setItem('token', data.token);
                history.push('/dashboard');
            } catch (error) {
                console.error(error.message);
            }
        };

        return (
            <div>
                <h2>Login</h2>
                <form onSubmit={handleSubmit}>
                    <div>
                        <label>Email</label>
                        <input
                            type="email"
                            value={email}
                            onChange={(e) => setEmail(e.target.value)}
                            required
                        />
                    </div>
                    <div>
                        <label>Password</label>
                        <input
                            type="password"
                            value={password}
                            onChange={(e) => setPassword(e.target.value)}
                            required
                        />
                    </div>
                    <button type="submit">Login</button>
                </form>
            </div>
        );
    }

    export default Login;
# Register Component:
import React, { useState } from 'react';
import { useHistory } from 'react-router-dom';
import { register } from '../../api/api';

function Register() {
    const [firstName, setFirstName] = useState('');
    const [lastName, setLastName] = useState('');
    const [email, setEmail] = useState('');
    const [phone, setPhone] = useState('');
    const [password, setPassword] = useState('');
    const history = useHistory();

    const handleSubmit = async (e) => {
        e.preventDefault();
        try {
            const { data } = await register({ firstName, lastName, email, phone, password });
            localStorage.setItem('token', data.token);
            history.push('/dashboard');
        } catch (error) {
            console.error(error.message);
        }
    };

    return (
        <div>
            <h2>Register</h2>
            <form onSubmit={handleSubmit}>
                <div>
                    <label>First Name</label>
                    <input
                        type="text"
                        value={firstName}
                        onChange={(e) => setFirstName(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Last Name</label>
                    <input
                        type="text"
                        value={lastName}
                        onChange={(e) => setLastName(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Email</label>
                    <input
                        type="email"
                        value={email}
                        onChange={(e) => setEmail(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Phone Number</label>
                    <input
                        type="text"
                        value={phone}
                        onChange={(e) => setPhone(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Password</label>
                    <input
                        type="password"
                        value={password}
                        onChange={(e) => setPassword(e.target.value)}
                        required
                    />
                </div>
                <button type="submit">Register</button>
            </form>
        </div>
    );
}

export default Register;
import React, { useState } from 'react';
import { useHistory } from 'react-router-dom';
import { register } from '../../api/api';

function Register() {
    const [firstName, setFirstName] = useState('');
    const [lastName, setLastName] = useState('');
    const [email, setEmail] = useState('');
    const [phone, setPhone] = useState('');
    const [password, setPassword] = useState('');
    const history = useHistory();

    const handleSubmit = async (e) => {
        e.preventDefault();
        try {
            const { data } = await register({ firstName, lastName, email, phone, password });
            localStorage.setItem('token', data.token);
            history.push('/dashboard');
        } catch (error) {
            console.error(error.message);
        }
    };

    return (
        <div>
            <h2>Register</h2>
            <form onSubmit={handleSubmit}>
                <div>
                    <label>First Name</label>
                    <input
                        type="text"
                        value={firstName}
                        onChange={(e) => setFirstName(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Last Name</label>
                    <input
                        type="text"
                        value={lastName}
                        onChange={(e) => setLastName(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Email</label>
                    <input
                        type="email"
                        value={email}
                        onChange={(e) => setEmail(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Phone Number</label>
                    <input
                        type="text"
                        value={phone}
                        onChange={(e) => setPhone(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Password</label>
                    <input
                        type="password"
                        value={password}
                        onChange={(e) => setPassword(e.target.value)}
                        required
                    />
                </div>
                <button type="submit">Register</button>
            </form>
        </div>
    );
}

export default Register;
import React, { useState } from 'react';
import { useHistory } from 'react-router-dom';
import { register } from '../../api/api';

function Register() {
    const [firstName, setFirstName] = useState('');
    const [lastName, setLastName] = useState('');
    const [email, setEmail] = useState('');
    const [phone, setPhone] = useState('');
    const [password, setPassword] = useState('');
    const history = useHistory();

    const handleSubmit = async (e) => {
        e.preventDefault();
        try {
            const { data } = await register({ firstName, lastName, email, phone, password });
            localStorage.setItem('token', data.token);
            history.push('/dashboard');
        } catch (error) {
            console.error(error.message);
        }
    };

    return (
        <div>
            <h2>Register</h2>
            <form onSubmit={handleSubmit}>
                <div>
                    <label>First Name</label>
                    <input
                        type="text"
                        value={firstName}
                        onChange={(e) => setFirstName(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Last Name</label>
                    <input
                        type="text"
                        value={lastName}
                        onChange={(e) => setLastName(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Email</label>
                    <input
                        type="email"
                        value={email}
                        onChange={(e) => setEmail(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Phone Number</label>
                    <input
                        type="text"
                        value={phone}
                        onChange={(e) => setPhone(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Password</label>
                    <input
                        type="password"
                        value={password}
                        onChange={(e) => setPassword(e.target.value)}
                        required
                    />
                </div>
                <button type="submit">Register</button>
            </form>
        </div>
    );
}

export default Register;

**Dashboard Component:**

    Create `src/components/Dashboard.js`:

    ```js
    import React, { useEffect, useState } from 'react';
    import { getProfile } from '../api/api';

    function Dashboard() {
        const [user, setUser] = useState(null);

        useEffect(() => {
            const fetchProfile = async () => {
                try {
                    const { data } = await getProfile();
                    setUser(data);
                } catch (error) {
                    console.error(error.message);
                }
            };

            fetchProfile();
        }, []);

        return (
            <div>
                <h2>Dashboard</h2>
                {user && (
                    <div>
                        <p>Welcome, {user.firstName} {user.lastName}</p>
                        <p>Email: {user.email}</p>
                        <p>Phone: {user.phone}</p>
                    </div>
                )}
            </div>
        );
    }

    export default Dashboard;
    # Profile Component:
    import React, { useEffect, useState } from 'react';
import { getProfile } from '../api/api';

function Profile() {
    const [user, setUser] = useState(null);

    useEffect(() => {
        const fetchProfile = async () => {
            try {
                const { data } = await getProfile();
                setUser(data);
            } catch (error) {
                console.error(error.message);
            }
        };

        fetchProfile();
    }, []);

    return (
        <div>
            <h2>Profile</h2>
            {user && (
                <div>
                    <p>First Name: {user.firstName}</p>
                    <p>Last Name: {user.lastName}</p>
                    <p>Email: {user.email}</p>
                    <p>Phone: {user.phone}</p>
                </div>
            )}
        </div>
    );
}

export default Profile;

**Property List Component:**

    Create `src/components/Properties/PropertyList.js`:

    ```js
    import React, { useEffect, useState } from 'react';
    import { getProperties } from '../../api/api';
    import { Link } from 'react-router-dom';

    function PropertyList() {
        const [properties, setProperties] = useState([]);

        useEffect(() => {
            const fetchProperties = async () => {
                try {
                    const { data } = await getProperties();
                    setProperties(data);
                } catch (error) {
                    console.error(error.message);
                }
            };

            fetchProperties();
        }, []);

        return (
            <div>
                <h2>Properties</h2>
                <ul>
                    {properties.map((property) => (
                        <li key={property._id}>
                            <Link to={`/properties/${property._id}`}>
                                {property.place} - {property.area}
                            </Link>
                        </li>
                    ))}
                </ul>
            </div>
        );
    }

    export default PropertyList;
 # Property Details Component:
 import React, { useEffect, useState } from 'react';
import { useParams } from 'react-router-dom';
import { getProperty, likeProperty } from '../../api/api';

function PropertyDetails() {
    const { id } = useParams();
    const [property, setProperty] = useState(null);

    useEffect(() => {
        const fetchProperty = async () => {
            try {
                const { data } = await getProperty(id);
                setProperty(data);
            } catch (error) {
                console.error(error.message);
            }
        };

        fetchProperty();
    }, [id]);

    const handleLike = async () => {
        try {
            const { data } = await likeProperty(id);
            setProperty(data);
        } catch (error) {
            console.error(error.message);
        }
    };

    return (
        <div>
            {property && (
                <div>
                    <h2>{property.place}</h2>
                    <p>Area: {property.area}</p>
                    <p>Bedrooms: {property.bedrooms}</p>
                    <p>Bathrooms: {property.bathrooms}</p>
                    <p>Nearby Hospitals: {property.nearbyHospitals}</p>
                    <p>Nearby Colleges: {property.nearbyColleges}</p>
                    <p>Likes: {property.likes}</p>
                    <button onClick={handleLike}>Like</button>
                </div>
            )}
        </div>
    );
}

export default PropertyDetails;

 **Add Property Component:**

    

    ```js
    import React, { useState } from 'react';
    import { useHistory } from 'react-router-dom';
    import { createProperty } from '../../api/api';

    function AddProperty() {
        const [place, setPlace] = useState('');
        const [area, setArea] = useState('');
        const [bedrooms, setBedrooms] = useState('');
        const [bathrooms, setBathrooms] = useState('');
        const [nearbyHospitals, setNearbyHospitals] = useState('');
        const [nearbyColleges, setNearbyColleges] = useState('');
        const history = useHistory();

        const handleSubmit = async (e) => {
            e.preventDefault();
            try {
                await createProperty({
                    place,
                    area,
                    bedrooms,
                    bathrooms,
                    nearbyHospitals,
                    nearbyColleges,
                });
                history.push('/properties');
            } catch (error) {
                console.error(error.message);
            }
        };

        return (
            <div>
                <h2>Add Property</h2>
                <form onSubmit={handleSubmit}>
                    <div>
                        <label>Place</label>
                        <input
                            type="text"
                            value={place}
                            onChange={(e) => setPlace(e.target.value)}
                            required
                        />
                    </div>
                    <div>
                        <label>Area</label>
                        <input
                            type="text"
                            value={area}
                            onChange={(e) => setArea(e.target.value)}
                            required
                        />
                    </div>
                    <div>
                        <label>Bedrooms</label>
                        <input
                            type="number"
                            value={bedrooms}
                            onChange={(e) => setBedrooms(e.target.value)}
                            required
                        />
                    </div>
                    <div>
                        <label>Bathrooms</label>
                        <input
                            type="number"
                            value={bathrooms}
                            onChange={(e) => setBathrooms(e.target.value)}
                            required
                        />
                    </div>
                    <div>
                        <label>Nearby Hospitals</label>
                        <input
                            type="text"
                            value={nearbyHospitals}
                            onChange={(e) => setNearbyHospitals(e.target.value)}
                            required
                        />
                    </div>
                    <div>
                        <label>Nearby Colleges</label>
                        <input
                            type="text"
                            value={nearbyColleges}
                            onChange={(e) => setNearbyColleges(e.target.value)}
                            required
                        />
                    </div>
                    <button type="submit">Add Property</button>
                </form>
            </div>
        );
    }

    export default AddProperty;
   #  Edit Property Component:
   import React, { useEffect, useState } from 'react';
import { useHistory, useParams } from 'react-router-dom';
import { getProperty, updateProperty } from '../../api/api';

function EditProperty() {
    const { id } = useParams();
    const [place, setPlace] = useState('');
    const [area, setArea] = useState('');
    const [bedrooms, setBedrooms] = useState('');
    const [bathrooms, setBathrooms] = useState('');
    const [nearbyHospitals, setNearbyHospitals] = useState('');
    const [nearbyColleges, setNearbyColleges] = useState('');
    const history = useHistory();

    useEffect(() => {
        const fetchProperty = async () => {
            try {
                const { data } = await getProperty(id);
                setPlace(data.place);
                setArea(data.area);
                setBedrooms(data.bedrooms);
                setBathrooms(data.bathrooms);
                setNearbyHospitals(data.nearbyHospitals);
                setNearbyColleges(data.nearbyColleges);
            } catch (error) {
                console.error(error.message);
            }
        };

        fetchProperty();
    }, [id]);

    const handleSubmit = async (e) => {
        e.preventDefault();
        try {
            await updateProperty(id, {
                place,
                area,
                bedrooms,
                bathrooms,
                nearbyHospitals,
                nearbyColleges,
            });
            history.push(`/properties/${id}`);
        } catch (error) {
            console.error(error.message);
        }
    };

    return (
        <div>
            <h2>Edit Property</h2>
            <form onSubmit={handleSubmit}>
                <div>
                    <label>Place</label>
                    <input
                        type="text"
                        value={place}
                        onChange={(e) => setPlace(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Area</label>
                    <input
                        type="text"
                        value={area}
                        onChange={(e) => setArea(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Bedrooms</label>
                    <input
                        type="number"
                        value={bedrooms}
                        onChange={(e) => setBedrooms(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Bathrooms</label>
                    <input
                        type="number"
                        value={bathrooms}
                        onChange={(e) => setBathrooms(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Nearby Hospitals</label>
                    <input
                        type="text"
                        value={nearbyHospitals}
                        onChange={(e) => setNearbyHospitals(e.target.value)}
                        required
                    />
                </div>
                <div>
                    <label>Nearby Colleges</label>
                    <input
                        type="text"
                        value={nearbyColleges}
                        onChange={(e) => setNearbyColleges(e.target.value)}
                        required
                    />
                </div>
                <button type="submit">Update Property</button>
            </form>
        </div>
    );
}

export default EditProperty;








   

