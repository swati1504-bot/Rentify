# Rentify
Backend Setup
# Initialize the Project:
mkdir rentify-backend
cd rentify-backend
npm init -y
npm install express mongoose bcryptjs jsonwebtoken dotenv cors
npm install --save-dev nodemon
# Project Structure:
rentify-backend/
├── config/
│   └── db.js
├── controllers/
│   ├── authController.js
│   ├── propertyController.js
│   └── userController.js
├── middleware/
│   └── auth.js
├── models/
│   ├── User.js
│   └── Property.js
├── routes/
│   ├── authRoutes.js
│   ├── propertyRoutes.js
│   └── userRoutes.js
├── .env
├── server.js
└── package.json
# Environment Variables:

PORT=3000
MONGO_URI=Rentify_database
JWT_SECRET=rentify_secret_key
# Database Configuration:

const mongoose = require('mongoose');
const dotenv = require('dotenv');

dotenv.config();

const connectDB = async () => {
    try {
        await mongoose.connect(process.env.MONGO_URI, {
            useNewUrlParser: true,
            useUnifiedTopology: true,
        });
        console.log('MongoDB connected');
    } catch (error) {
        console.error(error.message);
        process.exit(1);
    }
};

module.exports = connectDB;
# User Model:


const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');

const UserSchema = new mongoose.Schema({
    firstName: {
        type: String,
        required: true,
    },
    lastName: {
        type: String,
        required: true,
    },
    email: {
        type: String,
        required: true,
        unique: true,
    },
    phone: {
        type: String,
        required: true,
    },
    password: {
        type: String,
        required: true,
    },
});

UserSchema.pre('save', async function (next) {
    if (!this.isModified('password')) {
        return next();
    }
    const salt = await bcrypt.genSalt(10);
    this.password = await bcrypt.hash(this.password, salt);
    next();
});

UserSchema.methods.matchPassword = async function (enteredPassword) {
    return await bcrypt.compare(enteredPassword, this.password);
};

const User = mongoose.model('User', UserSchema);

module.exports = User;
# Property Model:
const mongoose = require('mongoose');

const PropertySchema = new mongoose.Schema({
    place: {
        type: String,
        required: true,
    },
    area: {
        type: String,
        required: true,
    },
    bedrooms: {
        type: Number,
        required: true,
    },
    bathrooms: {
        type: Number,
        required: true,
    },
    nearbyHospitals: {
        type: String,
        required: false,
    },
    nearbyColleges: {
        type: String,
        required: false,
    },
    user: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'User',
        required: true,
    },
    likes: {
        type: Number,
        default: 0,
    },
});

const Property = mongoose.model('Property', PropertySchema);

module.exports = Property;
# Auth Middleware:
const jwt = require('jsonwebtoken');
const User = require('../models/User');
const dotenv = require('dotenv');

dotenv.config();

const auth = async (req, res, next) => {
    const token = req.header('Authorization').replace('Bearer ', '');

    try {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        const user = await User.findById(decoded.id);

        if (!user) {
            return res.status(401).json({ message: 'Not authorized' });
        }

        req.user = user;
        next();
    } catch (error) {
        res.status(401).json({ message: 'Not authorized' });
    }
};

module.exports = auth;
# Auth Controller:
const jwt = require('jsonwebtoken');
const User = require('../models/User');
const dotenv = require('dotenv');

dotenv.config();

const generateToken = (id) => {
    return jwt.sign({ id }, process.env.JWT_SECRET, { expiresIn: '30d' });
};

exports.register = async (req, res) => {
    const { firstName, lastName, email, phone, password } = req.body;

    try {
        const user = await User.create({
            firstName,
            lastName,
            email,
            phone,
            password,
        });

        res.status(201).json({
            token: generateToken(user._id),
        });
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};

exports.login = async (req, res) => {
    const { email, password } = req.body;

    try {
        const user = await User.findOne({ email });

        if (user && (await user.matchPassword(password))) {
            res.json({
                token: generateToken(user._id),
            });
        } else {
            res.status(401).json({ message: 'Invalid email or password' });
        }
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};
# User Controller: 
const User = require('../models/User');

exports.getUserProfile = async (req, res) => {
    try {
        const user = await User.findById(req.user._id);
        res.json(user);
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};
# Property Controller:
const Property = require('../models/Property');

exports.createProperty = async (req, res) => {
    const { place, area, bedrooms, bathrooms, nearbyHospitals, nearbyColleges } = req.body;

    try {
        const property = await Property.create({
            place,
            area,
            bedrooms,
            bathrooms,
            nearbyHospitals,
            nearbyColleges,
            user: req.user._id,
        });

        res.status(201).json(property);
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};

exports.getProperties = async (req, res) => {
    try {
        const properties = await Property.find().populate('user', 'firstName lastName email phone');
        res.json(properties);
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};

exports.getProperty = async (req, res) => {
    try {
        const property = await Property.findById(req.params.id).populate('user', 'firstName lastName email phone');
        res.json(property);
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};

exports.updateProperty = async (req, res) => {
    try {
        const property = await Property.findByIdAndUpdate(req.params.id, req.body, { new: true });
        res.json(property);
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};

exports.deleteProperty = async (req, res) => {
    try {
        await Property.findByIdAndDelete(req.params.id);
        res.json({ message: 'Property deleted' });
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};

exports.likeProperty = async (req, res) => {
    try {
        const property = await Property.findById(req.params.id);
        property.likes += 1;
        await property.save();
        res.json(property);
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};
# Auth Routes:
const express = require('express');
const { register, login } = require('../controllers/authController');

const router = express.Router();

router.post('/register', register);
router.post('/login', login);

module.exports = router;
# User Routes:
const express = require('express');
const { getUserProfile } = require('../controllers/userController');
const auth = require('../middleware/auth');

const router = express.Router();

router.get('/profile', auth, getUserProfile);

module.exports = router;
# Property Routes:
const express = require('express');
const {
    createProperty,
    getProperties,
    getProperty,
    updateProperty,
    deleteProperty,
    likeProperty,
} = require('../controllers/propertyController');
const auth = require('../middleware/auth');

const router = express.Router();

router.route('/')
    .post(auth, createProperty)
    .get(getProperties);

router.route('/:id')
    .get(getProperty)
    .put(auth, updateProperty)
    .delete(auth, deleteProperty);

router.post('/:id/like', auth, likeProperty);

module.exports = router;
# Server Setup:
const express = require('express');
const dotenv = require('dotenv');
const cors = require('cors');
const connectDB = require('./config/db');
const authRoutes = require('./routes/authRoutes');
const userRoutes = require('./routes/userRoutes');
const propertyRoutes = require('./routes/propertyRoutes');

dotenv.config();

connectDB();

const app = express();

app.use(cors());
app.use(express.json());

app.use('/api/users', authRoutes);
app.use('/api/users', userRoutes);
app.use('/api/properties', propertyRoutes);

const PORT = process.env.PORT || 5000;

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
# Start the Server:
"scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
}
# Run the server:
npm run dev












