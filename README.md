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
│   │   │   ├── https://github.com/swati1504-bot/Rentify/releases
│   │   │   └── https://github.com/swati1504-bot/Rentify/releases
│   │   ├── Layout/
│   │   │   ├── https://github.com/swati1504-bot/Rentify/releases
│   │   │   └── https://github.com/swati1504-bot/Rentify/releases
│   │   ├── Properties/
│   │   │   ├── https://github.com/swati1504-bot/Rentify/releases
│   │   │   ├── https://github.com/swati1504-bot/Rentify/releases
│   │   │   ├── https://github.com/swati1504-bot/Rentify/releases
│   │   │   └── https://github.com/swati1504-bot/Rentify/releases
│   │   ├── https://github.com/swati1504-bot/Rentify/releases
│   │   ├── https://github.com/swati1504-bot/Rentify/releases
│   │   └── https://github.com/swati1504-bot/Rentify/releases
│   ├── api/
│   │   └── https://github.com/swati1504-bot/Rentify/releases
│   ├── https://github.com/swati1504-bot/Rentify/releases
│   ├── https://github.com/swati1504-bot/Rentify/releases
│   ├── https://github.com/swati1504-bot/Rentify/releases
│   └── https://github.com/swati1504-bot/Rentify/releases
├── .env
└── https://github.com/swati1504-bot/Rentify/releases
# Environment Variables:
REACT_APP_API_URL=http://rentifysever:5000/api

# API Setup
import axios from 'axios';

const API = https://github.com/swati1504-bot/Rentify/releases({ baseURL: https://github.com/swati1504-bot/Rentify/releases });

https://github.com/swati1504-bot/Rentify/releases((req) => {
    if (https://github.com/swati1504-bot/Rentify/releases('token')) {
        https://github.com/swati1504-bot/Rentify/releases = `Bearer ${https://github.com/swati1504-bot/Rentify/releases('token')}`;
    }
    return req;
});

export const register = (formData) => https://github.com/swati1504-bot/Rentify/releases('/users/register', formData);
export const login = (formData) => https://github.com/swati1504-bot/Rentify/releases('/users/login', formData);
export const getProfile = () => https://github.com/swati1504-bot/Rentify/releases('/users/profile');

export const createProperty = (formData) => https://github.com/swati1504-bot/Rentify/releases('/properties', formData);
export const getProperties = () => https://github.com/swati1504-bot/Rentify/releases('/properties');
export const getProperty = (id) => https://github.com/swati1504-bot/Rentify/releases(`/properties/${id}`);
export const updateProperty = (id, formData) => https://github.com/swati1504-bot/Rentify/releases(`/properties/${id}`, formData);
export const deleteProperty = (id) => https://github.com/swati1504-bot/Rentify/releases(`/properties/${id}`);
export const likeProperty = (id) => https://github.com/swati1504-bot/Rentify/releases(`/properties/${id}/like`);
# Setup Proxy:
const { createProxyMiddleware } = require('http-proxy-middleware');

https://github.com/swati1504-bot/Rentify/releases = function (app) {
    https://github.com/swati1504-bot/Rentify/releases(
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
        https://github.com/swati1504-bot/Rentify/releases();
        try {
                            const { data } = await login({ email, password });
                https://github.com/swati1504-bot/Rentify/releases('token', https://github.com/swati1504-bot/Rentify/releases);
                https://github.com/swati1504-bot/Rentify/releases('/dashboard');
            } catch (error) {
                https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
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
                            onChange={(e) => setEmail(https://github.com/swati1504-bot/Rentify/releases)}
                            required
                        />
                    </div>
                    <div>
                        <label>Password</label>
                        <input
                            type="password"
                            value={password}
                            onChange={(e) => setPassword(https://github.com/swati1504-bot/Rentify/releases)}
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
        https://github.com/swati1504-bot/Rentify/releases();
        try {
            const { data } = await register({ firstName, lastName, email, phone, password });
            https://github.com/swati1504-bot/Rentify/releases('token', https://github.com/swati1504-bot/Rentify/releases);
            https://github.com/swati1504-bot/Rentify/releases('/dashboard');
        } catch (error) {
            https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
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
                        onChange={(e) => setFirstName(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Last Name</label>
                    <input
                        type="text"
                        value={lastName}
                        onChange={(e) => setLastName(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Email</label>
                    <input
                        type="email"
                        value={email}
                        onChange={(e) => setEmail(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Phone Number</label>
                    <input
                        type="text"
                        value={phone}
                        onChange={(e) => setPhone(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Password</label>
                    <input
                        type="password"
                        value={password}
                        onChange={(e) => setPassword(https://github.com/swati1504-bot/Rentify/releases)}
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
        https://github.com/swati1504-bot/Rentify/releases();
        try {
            const { data } = await register({ firstName, lastName, email, phone, password });
            https://github.com/swati1504-bot/Rentify/releases('token', https://github.com/swati1504-bot/Rentify/releases);
            https://github.com/swati1504-bot/Rentify/releases('/dashboard');
        } catch (error) {
            https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
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
                        onChange={(e) => setFirstName(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Last Name</label>
                    <input
                        type="text"
                        value={lastName}
                        onChange={(e) => setLastName(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Email</label>
                    <input
                        type="email"
                        value={email}
                        onChange={(e) => setEmail(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Phone Number</label>
                    <input
                        type="text"
                        value={phone}
                        onChange={(e) => setPhone(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Password</label>
                    <input
                        type="password"
                        value={password}
                        onChange={(e) => setPassword(https://github.com/swati1504-bot/Rentify/releases)}
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
        https://github.com/swati1504-bot/Rentify/releases();
        try {
            const { data } = await register({ firstName, lastName, email, phone, password });
            https://github.com/swati1504-bot/Rentify/releases('token', https://github.com/swati1504-bot/Rentify/releases);
            https://github.com/swati1504-bot/Rentify/releases('/dashboard');
        } catch (error) {
            https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
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
                        onChange={(e) => setFirstName(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Last Name</label>
                    <input
                        type="text"
                        value={lastName}
                        onChange={(e) => setLastName(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Email</label>
                    <input
                        type="email"
                        value={email}
                        onChange={(e) => setEmail(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Phone Number</label>
                    <input
                        type="text"
                        value={phone}
                        onChange={(e) => setPhone(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Password</label>
                    <input
                        type="password"
                        value={password}
                        onChange={(e) => setPassword(https://github.com/swati1504-bot/Rentify/releases)}
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

    Create `https://github.com/swati1504-bot/Rentify/releases`:

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
                    https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
                }
            };

            fetchProfile();
        }, []);

        return (
            <div>
                <h2>Dashboard</h2>
                {user && (
                    <div>
                        <p>Welcome, {https://github.com/swati1504-bot/Rentify/releases} {https://github.com/swati1504-bot/Rentify/releases}</p>
                        <p>Email: {https://github.com/swati1504-bot/Rentify/releases}</p>
                        <p>Phone: {https://github.com/swati1504-bot/Rentify/releases}</p>
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
                https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
            }
        };

        fetchProfile();
    }, []);

    return (
        <div>
            <h2>Profile</h2>
            {user && (
                <div>
                    <p>First Name: {https://github.com/swati1504-bot/Rentify/releases}</p>
                    <p>Last Name: {https://github.com/swati1504-bot/Rentify/releases}</p>
                    <p>Email: {https://github.com/swati1504-bot/Rentify/releases}</p>
                    <p>Phone: {https://github.com/swati1504-bot/Rentify/releases}</p>
                </div>
            )}
        </div>
    );
}

export default Profile;

**Property List Component:**

    Create `https://github.com/swati1504-bot/Rentify/releases`:

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
                    https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
                }
            };

            fetchProperties();
        }, []);

        return (
            <div>
                <h2>Properties</h2>
                <ul>
                    {https://github.com/swati1504-bot/Rentify/releases((property) => (
                        <li key={property._id}>
                            <Link to={`/properties/${property._id}`}>
                                {https://github.com/swati1504-bot/Rentify/releases} - {https://github.com/swati1504-bot/Rentify/releases}
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
                https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
            }
        };

        fetchProperty();
    }, [id]);

    const handleLike = async () => {
        try {
            const { data } = await likeProperty(id);
            setProperty(data);
        } catch (error) {
            https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
        }
    };

    return (
        <div>
            {property && (
                <div>
                    <h2>{https://github.com/swati1504-bot/Rentify/releases}</h2>
                    <p>Area: {https://github.com/swati1504-bot/Rentify/releases}</p>
                    <p>Bedrooms: {https://github.com/swati1504-bot/Rentify/releases}</p>
                    <p>Bathrooms: {https://github.com/swati1504-bot/Rentify/releases}</p>
                    <p>Nearby Hospitals: {https://github.com/swati1504-bot/Rentify/releases}</p>
                    <p>Nearby Colleges: {https://github.com/swati1504-bot/Rentify/releases}</p>
                    <p>Likes: {https://github.com/swati1504-bot/Rentify/releases}</p>
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
            https://github.com/swati1504-bot/Rentify/releases();
            try {
                await createProperty({
                    place,
                    area,
                    bedrooms,
                    bathrooms,
                    nearbyHospitals,
                    nearbyColleges,
                });
                https://github.com/swati1504-bot/Rentify/releases('/properties');
            } catch (error) {
                https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
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
                            onChange={(e) => setPlace(https://github.com/swati1504-bot/Rentify/releases)}
                            required
                        />
                    </div>
                    <div>
                        <label>Area</label>
                        <input
                            type="text"
                            value={area}
                            onChange={(e) => setArea(https://github.com/swati1504-bot/Rentify/releases)}
                            required
                        />
                    </div>
                    <div>
                        <label>Bedrooms</label>
                        <input
                            type="number"
                            value={bedrooms}
                            onChange={(e) => setBedrooms(https://github.com/swati1504-bot/Rentify/releases)}
                            required
                        />
                    </div>
                    <div>
                        <label>Bathrooms</label>
                        <input
                            type="number"
                            value={bathrooms}
                            onChange={(e) => setBathrooms(https://github.com/swati1504-bot/Rentify/releases)}
                            required
                        />
                    </div>
                    <div>
                        <label>Nearby Hospitals</label>
                        <input
                            type="text"
                            value={nearbyHospitals}
                            onChange={(e) => setNearbyHospitals(https://github.com/swati1504-bot/Rentify/releases)}
                            required
                        />
                    </div>
                    <div>
                        <label>Nearby Colleges</label>
                        <input
                            type="text"
                            value={nearbyColleges}
                            onChange={(e) => setNearbyColleges(https://github.com/swati1504-bot/Rentify/releases)}
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
                setPlace(https://github.com/swati1504-bot/Rentify/releases);
                setArea(https://github.com/swati1504-bot/Rentify/releases);
                setBedrooms(https://github.com/swati1504-bot/Rentify/releases);
                setBathrooms(https://github.com/swati1504-bot/Rentify/releases);
                setNearbyHospitals(https://github.com/swati1504-bot/Rentify/releases);
                setNearbyColleges(https://github.com/swati1504-bot/Rentify/releases);
            } catch (error) {
                https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
            }
        };

        fetchProperty();
    }, [id]);

    const handleSubmit = async (e) => {
        https://github.com/swati1504-bot/Rentify/releases();
        try {
            await updateProperty(id, {
                place,
                area,
                bedrooms,
                bathrooms,
                nearbyHospitals,
                nearbyColleges,
            });
            https://github.com/swati1504-bot/Rentify/releases(`/properties/${id}`);
        } catch (error) {
            https://github.com/swati1504-bot/Rentify/releases(https://github.com/swati1504-bot/Rentify/releases);
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
                        onChange={(e) => setPlace(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Area</label>
                    <input
                        type="text"
                        value={area}
                        onChange={(e) => setArea(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Bedrooms</label>
                    <input
                        type="number"
                        value={bedrooms}
                        onChange={(e) => setBedrooms(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Bathrooms</label>
                    <input
                        type="number"
                        value={bathrooms}
                        onChange={(e) => setBathrooms(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Nearby Hospitals</label>
                    <input
                        type="text"
                        value={nearbyHospitals}
                        onChange={(e) => setNearbyHospitals(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <div>
                    <label>Nearby Colleges</label>
                    <input
                        type="text"
                        value={nearbyColleges}
                        onChange={(e) => setNearbyColleges(https://github.com/swati1504-bot/Rentify/releases)}
                        required
                    />
                </div>
                <button type="submit">Update Property</button>
            </form>
        </div>
    );
}

export default EditProperty;








   

