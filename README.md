// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
import "@openzeppelin/contracts/utils/Base64.sol";
import "@openzeppelin/contracts/utils/Strings.sol";

/**
 * @title Shackled - On-Chain 3D Rendering Engine
 * @dev A revolutionary smart contract that generates and renders 3D graphics entirely on-chain
 * @author Shackled Team
 */
contract Shackled is ERC721, Ownable {
    using Counters for Counters.Counter;
    using Strings for uint256;

    // Token counter for unique NFTs
    Counters.Counter private _tokenIdCounter;

    // 3D Object structure
    struct Vertex3D {
        int256 x;
        int256 y;
        int256 z;
    }

    struct Face3D {
        uint256 v1;
        uint256 v2;
        uint256 v3;
        string color;
    }

    struct Object3D {
        string name;
        Vertex3D[] vertices;
        Face3D[] faces;
        uint256 timestamp;
        address creator;
    }

    // Rendering parameters
    struct RenderParams {
        int256 cameraX;
        int256 cameraY;
        int256 cameraZ;
        int256 lightX;
        int256 lightY;
        int256 lightZ;
        uint256 fov;
        string backgroundColor;
    }

    // Storage mappings
    mapping(uint256 => Object3D) private _objects;
    mapping(uint256 => RenderParams) private _renderParams;
    mapping(address => uint256[]) private _creatorObjects;

    // Events
    event Object3DCreated(uint256 indexed tokenId, address indexed creator, string name);
    event Object3DRendered(uint256 indexed tokenId, string svgOutput);
    event RenderParamsUpdated(uint256 indexed tokenId, address indexed updater);

    // Constants for rendering calculations
    int256 private constant SCALE_FACTOR = 1000;
    int256 private constant PROJECTION_DISTANCE = 800;

    constructor() ERC721("Shackled 3D Engine", "SHCKLD") {}

    /**
     * @dev Core Function 1: Create a 3D object with vertices and faces
     * @param name Name of the 3D object
     * @param vertices Array of 3D vertices
     * @param faces Array of faces connecting vertices
     * @param renderParams Initial rendering parameters
     */
    function create3DObject(
        string memory name,
        Vertex3D[] memory vertices,
        Face3D[] memory faces,
        RenderParams memory renderParams
    ) external returns (uint256) {
        require(vertices.length >= 3, "Minimum 3 vertices required");
        require(faces.length >= 1, "Minimum 1 face required");
        require(bytes(name).length > 0, "Name cannot be empty");

        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();

        // Store the 3D object
        Object3D storage newObject = _objects[tokenId];
        newObject.name = name;
        newObject.timestamp = block.timestamp;
        newObject.creator = msg.sender;

        // Store vertices
        for (uint256 i = 0; i < vertices.length; i++) {
            newObject.vertices.push(vertices[i]);
        }

        // Store faces with validation
        for (uint256 i = 0; i < faces.length; i++) {
            require(faces[i].v1 < vertices.length, "Invalid vertex index v1");
            require(faces[i].v2 < vertices.length, "Invalid vertex index v2");
            require(faces[i].v3 < vertices.length, "Invalid vertex index v3");
            newObject.faces.push(faces[i]);
        }

        // Store render parameters
        _renderParams[tokenId] = renderParams;
        
        // Track creator's objects
        _creatorObjects[msg.sender].push(tokenId);

        // Mint NFT to creator
        _safeMint(msg.sender, tokenId);

        emit Object3DCreated(tokenId, msg.sender, name);
        return tokenId;
    }

    /**
     * @dev Core Function 2: Render 3D object to SVG format on-chain
     * @param tokenId ID of the 3D object to render
     * @return svgOutput Complete SVG representation of the 3D object
     */
    function render3DToSVG(uint256 tokenId) external view returns (string memory svgOutput) {
       // SPDX-License-Identifier: MIT
// SPDX-License-Identifier: MIT
// SPDX-License-Identifier: MIT
--> shackled.sol:133:3:

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
import "@openzeppelin/contracts/utils/Base64.sol";
import "@openzeppelin/contracts/utils/Strings.sol";

/**
 * @title Shackled - On-Chain 3D Rendering Engine
 * @dev A revolutionary smart contract that generates and renders 3D graphics entirely on-chain
 * @author Shackled Team
 */
contract Shackled is ERC721, Ownable {
    using Counters for Counters.Counter;
    using Strings for uint256;

    // Token counter for unique NFTs
    Counters.Counter private _tokenIdCounter;

    // 3D Object structure
    struct Vertex3D {
        int256 x;
        int256 y;
        int256 z;
    }

    struct Face3D {
        uint256 v1;
        uint256 v2;
        uint256 v3;
        string color;
    }

    struct Object3D {
        string name;
        Vertex3D[] vertices;
        Face3D[] faces;
        uint256 timestamp;
        address creator;
    }

    // Rendering parameters
    struct RenderParams {
        int256 cameraX;
        int256 cameraY;
        int256 cameraZ;
        int256 lightX;
        int256 lightY;
        int256 lightZ;
        uint256 fov;
        string backgroundColor;
    }

    // Storage mappings
    mapping(uint256 => Object3D) private _objects;
    mapping(uint256 => RenderParams) private _renderParams;
    mapping(address => uint256[]) private _creatorObjects;

    // Events
    event Object3DCreated(uint256 indexed tokenId, address indexed creator, string name);
    event Object3DRendered(uint256 indexed tokenId, string svgOutput);
    event RenderParamsUpdated(uint256 indexed tokenId, address indexed updater);

    // Constants for rendering calculations
    int256 private constant SCALE_FACTOR = 1000;
    int256 private constant PROJECTION_DISTANCE = 800;

    constructor() ERC721("Shackled 3D Engine", "SHCKLD") {}

    /**
     * @dev Core Function 1: Create a 3D object with vertices and faces
     * @param name Name of the 3D object
     * @param vertices Array of 3D vertices
     * @param faces Array of faces connecting vertices
     * @param renderParams Initial rendering parameters
     */
    function create3DObject(
        string memory name,
        Vertex3D[] memory vertices,
        Face3D[] memory faces,
        RenderParams memory renderParams
    ) external returns (uint256) {
        require(vertices.length >= 3, "Minimum 3 vertices required");
        require(faces.length >= 1, "Minimum 1 face required");
        require(bytes(name).length > 0, "Name cannot be empty");

        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();

        // Store the 3D object
        Object3D storage newObject = _objects[tokenId];
        newObject.name = name;
        newObject.timestamp = block.timestamp;
        newObject.creator = msg.sender;

        // Store vertices
        for (uint256 i = 0; i < vertices.length; i++) {
            newObject.vertices.push(vertices[i]);
        }

        // Store faces with validation
        for (uint256 i = 0; i < faces.length; i++) {
            require(faces[i].v1 < vertices.length, "Invalid vertex index v1");
            require(faces[i].v2 < vertices.length, "Invalid vertex index v2");
            require(faces[i].v3 < vertices.length, "Invalid vertex index v3");
            newObject.faces.push(faces[i]);
        }

        // Store render parameters
        _renderParams[tokenId] = renderParams;
        
        // Track creator's objects
        _creatorObjects[msg.sender].push(tokenId);

        // Mint NFT to creator
        _safeMint(msg.sender, tokenId);

        emit Object3DCreated(tokenId, msg.sender, name);
        return tokenId;
    }

    /**
     * @dev Core Function 2: Render 3D object to SVG format on-chain
     * @param tokenId ID of the 3D object to render
     * @return svgOutput Complete SVG representation of the 3D object
     */
    function render3DToSVG(uint256 tokenId) external view returns (string memory svgOutput) {
        require(_exists(tokenId), "Token does not exist");
        
        Object3D storage obj = _objects[tokenId];
        RenderParams storage params = _renderParams[tokenId];

        // Start SVG construction
        string memory svgHeader = string(abi.encodePacked(
            '<svg width="800" height="600" xmlns="http://www.w3.org/2000/svg">',
            '<rect width="100%" height="100%" fill="', params.backgroundColor, '"/>',
            '<g transform="translate(400,300)">'
        ));

        string memory faces = "";
        
        // Render each face
        for (uint256 i = 0; i < obj.faces.length; i++) {
            Face3D memory face = obj.faces[i];
            
            // Get 3D vertices
            Vertex3D memory v1 = obj.vertices[face.v1];
            Vertex3D memory v2 = obj.vertices[face.v2];
            Vertex3D memory v3 = obj.vertices[face.v3];

            // Project 3D to 2D coordinates
            (int256 x1, int256 y1) = project3DTo2D(v1, params);
            (int256 x2, int256 y2) = project3DTo2D(v2, params);
            (int256 x3, int256 y3) = project3DTo2D(v3, params);

            // Calculate lighting (simple dot product with light direction)
            int256 lighting = calculateLighting(v1, v2, v3, params);
            string memory opacity = _calculateOpacity(lighting);

            // Create triangle polygon
            faces = string(abi.encodePacked(
                faces,
                '<polygon points="',
                _int256ToString(x1), ',', _int256ToString(y1), ' ',
                _int256ToString(x2), ',', _int256ToString(y2), ' ',
                _int256ToString(x3), ',', _int256ToString(y3),
                '" fill="', face.color, '" opacity="', opacity, '" stroke="#000" stroke-width="0.5"/>'
            ));
        }

        svgOutput = string(abi.encodePacked(
            svgHeader,
            faces,
            '</g></svg>'
        ));

        return svgOutput;
    }

    /**
     * @dev Core Function 3: Update rendering parameters for dynamic 3D manipulation
     * @param tokenId ID of the 3D object
     * @param newParams New rendering parameters
     */
    function updateRenderParams(uint256 tokenId, RenderParams memory newParams) external {
        require(_exists(tokenId), "Token does not exist");
        require(
            ownerOf(tokenId) == msg.sender || owner() == msg.sender,
            "Not authorized to update render params"
        );

        _renderParams[tokenId] = newParams;
        emit RenderParamsUpdated(tokenId, msg.sender);
    }

    /**
     * @dev Project 3D coordinates to 2D screen coordinates
     */
    function project3DTo2D(Vertex3D memory vertex, RenderParams memory params) 
        internal 
        pure 
        returns (int256 x2d, int256 y2d) 
    {
        // Translate relative to camera
        int256 relativeX = vertex.x - params.cameraX;
        int256 relativeY = vertex.y - params.cameraY;
        int256 relativeZ = vertex.z - params.cameraZ;

        // Perspective projection
        if (relativeZ == 0) relativeZ = 1; // Avoid division by zero
        
        x2d = (relativeX * PROJECTION_DISTANCE) / (relativeZ + PROJECTION_DISTANCE);
        y2d = (relativeY * PROJECTION_DISTANCE) / (relativeZ + PROJECTION_DISTANCE);
    }

    /**
     * @dev Calculate basic lighting using face normal and light direction
     */
    function calculateLighting(
        Vertex3D memory v1, 
        Vertex3D memory v2, 
        Vertex3D memory v3,
        RenderParams memory params
    ) internal pure returns (int256) {
        // Calculate face normal (simplified)
        int256 nx = (v2.y - v1.y) * (v3.z - v1.z) - (v2.z - v1.z) * (v3.y - v1.y);
        int256 ny = (v2.z - v1.z) * (v3.x - v1.x) - (v2.x - v1.x) * (v3.z - v1.z);
        int256 nz = (v2.x - v1.x) * (v3.y - v1.y) - (v2.y - v1.y) * (v3.x - v1.x);

        // Light direction (normalized conceptually)
        int256 lightDot = nx * params.lightX + ny * params.lightY + nz * params.lightZ;
        
        return lightDot > 0 ? lightDot / 1000 : 0; // Simplified lighting calculation
    }

    /**
     * @dev Helper function to calculate opacity based on lighting
     */
    function _calculateOpacity(int256 lighting) internal pure returns (string memory) {
        if (lighting > 800) return "1.0";
        if (lighting > 600) return "0.8";
        if (lighting > 400) return "0.6";
        if (lighting > 200) return "0.4";
        return "0.2";
    }

    /**
     * @dev Convert int256 to string
     */
    function _int256ToString(int256 value) internal pure returns (string memory) {
        if (value == 0) return "0";
        
        bool negative = value < 0;
        if (negative) value = -value;
        
        uint256 temp = uint256(value);
        uint256 digits;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }
        
        bytes memory buffer = new bytes(digits + (negative ? 1 : 0));
        uint256 index = digits + (negative ? 1 : 0);
        temp = uint256(value);
        
        while (temp != 0) {
            index--;
            buffer[index] = bytes1(uint8(48 + temp % 10));
            temp /= 10;
        }
        
        if (negative) {
            buffer[0] = "-";
        }
        
        return string(buffer);
    }

    // View functions
    function getObject3D(uint256 tokenId) external view returns (Object3D memory) {
        require(_exists(tokenId), "Token does not exist");
        return _objects[tokenId];
    }

    function getRenderParams(uint256 tokenId) external view returns (RenderParams memory) {
        require(_exists(tokenId), "Token does not exist");
        return _renderParams[tokenId];
    }

    function getCreatorObjects(address creator) external view returns (uint256[] memory) {
        return _creatorObjects[creator];
    }

    function tokenURI(uint256 tokenId) public view override returns (string memory) {
        require(_exists(tokenId), "Token does not exist");
        
        Object3D memory obj = _objects[tokenId];
        string memory svg = this.render3DToSVG(tokenId);
        
        string memory json = Base64.encode(
            bytes(
                string(
                    abi.encodePacked(
                        '{"name": "', obj.name, '",',
                        '"description": "On-chain 3D rendered object from Shackled Engine",',
                        '"image": "data:image/svg+xml;base64,', Base64.encode(bytes(svg)), '",',
                        '"attributes": [',
                        '{"trait_type": "Creator", "value": "', Strings.toHexString(uint160(obj.creator), 20), '"},',
                        '{"trait_type": "Vertices", "value": ', Strings.toString(obj.vertices.length), '},',
                        '{"trait_type": "Faces", "value": ', Strings.toString(obj.faces.length), '},',
                        '{"trait_type": "Created", "value": ', Strings.toString(obj.timestamp), '}',
                        ']}'
                    )
                )
            )
        );
        
        return string(abi.encodePacked("data:application/json;base64,", json));
    }
}
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
import "@openzeppelin/contracts/utils/Base64.sol";
import "@openzeppelin/contracts/utils/Strings.sol";

/**
 * @title Shackled - On-Chain 3D Rendering Engine
 * @dev A revolutionary smart contract that generates and renders 3D graphics entirely on-chain
 * @author Shackled Team
 */
contract Shackled is ERC721, Ownable {
    using Counters for Counters.Counter;
    using Strings for uint256;

    // Token counter for unique NFTs
    Counters.Counter private _tokenIdCounter;

    // 3D Object structure
    struct Vertex3D {
        int256 x;
        int256 y;
        int256 z;
    }

    struct Face3D {
        uint256 v1;
        uint256 v2;
        uint256 v3;
        string color;
    }

    struct Object3D {
        string name;
        Vertex3D[] vertices;
        Face3D[] faces;
        uint256 timestamp;
        address creator;
    }

    // Rendering parameters
    struct RenderParams {
        int256 cameraX;
        int256 cameraY;
        int256 cameraZ;
        int256 lightX;
        int256 lightY;
        int256 lightZ;
        uint256 fov;
        string backgroundColor;
    }

    // Storage mappings
    mapping(uint256 => Object3D) private _objects;
    mapping(uint256 => RenderParams) private _renderParams;
    mapping(address => uint256[]) private _creatorObjects;

    // Events
    event Object3DCreated(uint256 indexed tokenId, address indexed creator, string name);
    event Object3DRendered(uint256 indexed tokenId, string svgOutput);
    event RenderParamsUpdated(uint256 indexed tokenId, address indexed updater);

    // Constants for rendering calculations
    int256 private constant SCALE_FACTOR = 1000;
    int256 private constant PROJECTION_DISTANCE = 800;

    constructor() ERC721("Shackled 3D Engine", "SHCKLD") {}

    /**
     * @dev Core Function 1: Create a 3D object with vertices and faces
     * @param name Name of the 3D object
     * @param vertices Array of 3D vertices
     * @param faces Array of faces connecting vertices
     * @param renderParams Initial rendering parameters
     */
    function create3DObject(
        string memory name,
        Vertex3D[] memory vertices,
        Face3D[] memory faces,
        RenderParams memory renderParams
    ) external returns (uint256) {
        require(vertices.length >= 3, "Minimum 3 vertices required");
        require(faces.length >= 1, "Minimum 1 face required");
        require(bytes(name).length > 0, "Name cannot be empty");

        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();

        // Store the 3D object
        Object3D storage newObject = _objects[tokenId];
        newObject.name = name;
        newObject.timestamp = block.timestamp;
        newObject.creator = msg.sender;

        // Store vertices
        for (uint256 i = 0; i < vertices.length; i++) {
            newObject.vertices.push(vertices[i]);
        }

        // Store faces with validation
        for (uint256 i = 0; i < faces.length; i++) {
            require(faces[i].v1 < vertices.length, "Invalid vertex index v1");
            require(faces[i].v2 < vertices.length, "Invalid vertex index v2");
            require(faces[i].v3 < vertices.length, "Invalid vertex index v3");
            newObject.faces.push(faces[i]);
        }

        // Store render parameters
        _renderParams[tokenId] = renderParams;
        
        // Track creator's objects
        _creatorObjects[msg.sender].push(tokenId);

        // Mint NFT to creator
        _safeMint(msg.sender, tokenId);

        emit Object3DCreated(tokenId, msg.sender, name);
        return tokenId;
    }

    /**
     * @dev Core Function 2: Render 3D object to SVG format on-chain
     * @param tokenId ID of the 3D object to render
     * @return svgOutput Complete SVG representation of the 3D object
     */
    function render3DToSVG(uint256 tokenId) external view returns (string memory svgOutput) {
        require(_exists(tokenId), "Token does not exist");
        
        Object3D storage obj = _objects[tokenId];
        RenderParams storage params = _renderParams[tokenId];

        // Start SVG construction
        string memory svgHeader = string(abi.encodePacked(
            '<svg width="800" height="600" xmlns="http://www.w3.org/2000/svg">',
            '<rect width="100%" height="100%" fill="', params.backgroundColor, '"/>',
            '<g transform="translate(400,300)">'
        ));

        string memory faces = "";
        
        // Render each face
        for (uint256 i = 0; i < obj.faces.length; i++) {
            Face3D memory face = obj.faces[i];
            
            // Get 3D vertices
            Vertex3D memory v1 = obj.vertices[face.v1];
            Vertex3D memory v2 = obj.vertices[face.v2];
            Vertex3D memory v3 = obj.vertices[face.v3];

            // Project 3D to 2D coordinates
            (int256 x1, int256 y1) = project3DTo2D(v1, params);
            (int256 x2, int256 y2) = project3DTo2D(v2, params);
            (int256 x3, int256 y3) = project3DTo2D(v3, params);

            // Calculate lighting (simple dot product with light direction)
            int256 lighting = calculateLighting(v1, v2, v3, params);
            string memory opacity = _calculateOpacity(lighting);

            // Create triangle polygon
            faces = string(abi.encodePacked(
                faces,
                '<polygon points="',
                _int256ToString(x1), ',', _int256ToString(y1), ' ',
                _int256ToString(x2), ',', _int256ToString(y2), ' ',
                _int256ToString(x3), ',', _int256ToString(y3),
                '" fill="', face.color, '" opacity="', opacity, '" stroke="#000" stroke-width="0.5"/>'
            ));
        }

        svgOutput = string(abi.encodePacked(
            svgHeader,
            faces,
            '</g></svg>'
        ));

        return svgOutput;
    }

    /**
     * @dev Core Function 3: Update rendering parameters for dynamic 3D manipulation
     * @param tokenId ID of the 3D object
     * @param newParams New rendering parameters
     */
    function updateRenderParams(uint256 tokenId, RenderParams memory newParams) external {
        require(_exists(tokenId), "Token does not exist");
        require(
            ownerOf(tokenId) == msg.sender || owner() == msg.sender,
            "Not authorized to update render params"
        );

        _renderParams[tokenId] = newParams;
        emit RenderParamsUpdated(tokenId, msg.sender);
    }

    /**
     * @dev Project 3D coordinates to 2D screen coordinates
     */
    function project3DTo2D(Vertex3D memory vertex, RenderParams memory params) 
        internal 
        pure 
        returns (int256 x2d, int256 y2d) 
    {
        // Translate relative to camera
        int256 relativeX = vertex.x - params.cameraX;
        int256 relativeY = vertex.y - params.cameraY;
        int256 relativeZ = vertex.z - params.cameraZ;

        // Perspective projection
        if (relativeZ == 0) relativeZ = 1; // Avoid division by zero
        
        x2d = (relativeX * PROJECTION_DISTANCE) / (relativeZ + PROJECTION_DISTANCE);
        y2d = (relativeY * PROJECTION_DISTANCE) / (relativeZ + PROJECTION_DISTANCE);
    }

    /**
     * @dev Calculate basic lighting using face normal and light direction
     */
    function calculateLighting(
        Vertex3D memory v1, 
        Vertex3D memory v2, 
        Vertex3D memory v3,
        RenderParams memory params
    ) internal pure returns (int256) {
        // Calculate face normal (simplified)
        int256 nx = (v2.y - v1.y) * (v3.z - v1.z) - (v2.z - v1.z) * (v3.y - v1.y);
        int256 ny = (v2.z - v1.z) * (v3.x - v1.x) - (v2.x - v1.x) * (v3.z - v1.z);
        int256 nz = (v2.x - v1.x) * (v3.y - v1.y) - (v2.y - v1.y) * (v3.x - v1.x);

        // Light direction (normalized conceptually)
        int256 lightDot = nx * params.lightX + ny * params.lightY + nz * params.lightZ;
        
        return lightDot > 0 ? lightDot / 1000 : 0; // Simplified lighting calculation
    }

    /**
     * @dev Helper function to calculate opacity based on lighting
     */
    function _calculateOpacity(int256 lighting) internal pure returns (string memory) {
        if (lighting > 800) return "1.0";
        if (lighting > 600) return "0.8";
        if (lighting > 400) return "0.6";
        if (lighting > 200) return "0.4";
        return "0.2";
    }

    /**
     * @dev Convert int256 to string
     */
    function _int256ToString(int256 value) internal pure returns (string memory) {
        if (value == 0) return "0";
        
        bool negative = value < 0;
        if (negative) value = -value;
        
        uint256 temp = uint256(value);
        uint256 digits;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }
        
        bytes memory buffer = new bytes(digits + (negative ? 1 : 0));
        uint256 index = digits + (negative ? 1 : 0);
        temp = uint256(value);
        
        while (temp != 0) {
            index--;
            buffer[index] = bytes1(uint8(48 + temp % 10));
            temp /= 10;
        }
        
        if (negative) {
            buffer[0] = "-";
        }
        
        return string(buffer);
    }

    // View functions
    function getObject3D(uint256 tokenId) external view returns (Object3D memory) {
        require(_exists(tokenId), "Token does not exist");
        return _objects[tokenId];
    }

    function getRenderParams(uint256 tokenId) external view returns (RenderParams memory) {
        require(_exists(tokenId), "Token does not exist");
        return _renderParams[tokenId];
    }

    function getCreatorObjects(address creator) external view returns (uint256[] memory) {
        return _creatorObjects[creator];
    }

    function tokenURI(uint256 tokenId) public view override returns (string memory) {
        require(_exists(tokenId), "Token does not exist");
        
        Object3D memory obj = _objects[tokenId];
        string memory svg = this.render3DToSVG(tokenId);
        
        string memory json = Base64.encode(
            bytes(
                string(
                    abi.encodePacked(
                        '{"name": "', obj.name, '",',
                        '"description": "On-chain 3D rendered object from Shackled Engine",',
                        '"image": "data:image/svg+xml;base64,', Base64.encode(bytes(svg)), '",',
                        '"attributes": [',
                        '{"trait_type": "Creator", "value": "', Strings.toHexString(uint160(obj.creator), 20), '"},',
                        '{"trait_type": "Vertices", "value": ', Strings.toString(obj.vertices.length), '},',
                        '{"trait_type": "Faces", "value": ', Strings.toString(obj.faces.length), '},',
                        '{"trait_type": "Created", "value": ', Strings.toString(obj.timestamp), '}',
                        ']}'
                    )
                )
            )
        );
        
        return string(abi.encodePacked("data:application/json;base64,", json));
    }
}

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
import "@openzeppelin/contracts/utils/Base64.sol";
import "@openzeppelin/contracts/utils/Strings.sol";

/**
 * @title Shackled - On-Chain 3D Rendering Engine
 * @dev A revolutionary smart contract that generates and renders 3D graphics entirely on-chain
 * @author Shackled Team
 */
contract Shackled is ERC721, Ownable {
    using Counters for Counters.Counter;
    using Strings for uint256;

    // Token counter for unique NFTs
    Counters.Counter private _tokenIdCounter;

    // 3D Object structure
    struct Vertex3D {
        int256 x;
        int256 y;
        int256 z;
    }

    struct Face3D {
        uint256 v1;
        uint256 v2;
        uint256 v3;
        string color;
    }

    struct Object3D {
        string name;
        Vertex3D[] vertices;
        Face3D[] faces;
        uint256 timestamp;
        address creator;
    }

    // Rendering parameters
    struct RenderParams {
        int256 cameraX;
        int256 cameraY;
        int256 cameraZ;
        int256 lightX;
        int256 lightY;
        int256 lightZ;
        uint256 fov;
        string backgroundColor;
    }

    // Storage mappings
    mapping(uint256 => Object3D) private _objects;
    mapping(uint256 => RenderParams) private _renderParams;
    mapping(address => uint256[]) private _creatorObjects;

    // Events
    event Object3DCreated(uint256 indexed tokenId, address indexed creator, string name);
    event Object3DRendered(uint256 indexed tokenId, string svgOutput);
    event RenderParamsUpdated(uint256 indexed tokenId, address indexed updater);

    // Constants for rendering calculations
    int256 private constant SCALE_FACTOR = 1000;
    int256 private constant PROJECTION_DISTANCE = 800;

    constructor() ERC721("Shackled 3D Engine", "SHCKLD") {}

    /**
     * @dev Core Function 1: Create a 3D object with vertices and faces
     * @param name Name of the 3D object
     * @param vertices Array of 3D vertices
     * @param faces Array of faces connecting vertices
     * @param renderParams Initial rendering parameters
     */
    function create3DObject(
        string memory name,
        Vertex3D[] memory vertices,
        Face3D[] memory faces,
        RenderParams memory renderParams
    ) external returns (uint256) {
        require(vertices.length >= 3, "Minimum 3 vertices required");
        require(faces.length >= 1, "Minimum 1 face required");
        require(bytes(name).length > 0, "Name cannot be empty");

        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();

        // Store the 3D object
        Object3D storage newObject = _objects[tokenId];
        newObject.name = name;
        newObject.timestamp = block.timestamp;
        newObject.creator = msg.sender;

        // Store vertices
        for (uint256 i = 0; i < vertices.length; i++) {
            newObject.vertices.push(vertices[i]);
        }

        // Store faces with validation
        for (uint256 i = 0; i < faces.length; i++) {
            require(faces[i].v1 < vertices.length, "Invalid vertex index v1");
            require(faces[i].v2 < vertices.length, "Invalid vertex index v2");
            require(faces[i].v3 < vertices.length, "Invalid vertex index v3");
            newObject.faces.push(faces[i]);
        }

        // Store render parameters
        _renderParams[tokenId] = renderParams;
        
        // Track creator's objects
        _creatorObjects[msg.sender].push(tokenId);

        // Mint NFT to creator
        _safeMint(msg.sender, tokenId);

        emit Object3DCreated(tokenId, msg.sender, name);
        return tokenId;
    }

    /**
     * @dev Core Function 2: Render 3D object to SVG format on-chain
     * @param tokenId ID of the 3D object to render
     * @return svgOutput Complete SVG representation of the 3D object
     */
    function render3DToSVG(uint256 tokenId) external view returns (string memory svgOutput) {
        require(_exists(tokenId), "Token does not exist");
        
        Object3D storage obj = _objects[tokenId];
        RenderParams storage params = _renderParams[tokenId];

        // Start SVG construction
        string memory svgHeader = string(abi.encodePacked(
            '<svg width="800" height="600" xmlns="http://www.w3.org/2000/svg">',
            '<rect width="100%" height="100%" fill="', params.backgroundColor, '"/>',
            '<g transform="translate(400,300)">'
        ));

        string memory faces = "";
        
        // Render each face
        for (uint256 i = 0; i < obj.faces.length; i++) {
            Face3D memory face = obj.faces[i];
            
            // Get 3D vertices
            Vertex3D memory v1 = obj.vertices[face.v1];
            Vertex3D memory v2 = obj.vertices[face.v2];
            Vertex3D memory v3 = obj.vertices[face.v3];

            // Project 3D to 2D coordinates
            (int256 x1, int256 y1) = project3DTo2D(v1, params);
            (int256 x2, int256 y2) = project3DTo2D(v2, params);
            (int256 x3, int256 y3) = project3DTo2D(v3, params);

            // Calculate lighting (simple dot product with light direction)
            int256 lighting = calculateLighting(v1, v2, v3, params);
            string memory opacity = _calculateOpacity(lighting);

            // Create triangle polygon
            faces = string(abi.encodePacked(
                faces,
                '<polygon points="',
                _int256ToString(x1), ',', _int256ToString(y1), ' ',
                _int256ToString(x2), ',', _int256ToString(y2), ' ',
                _int256ToString(x3), ',', _int256ToString(y3),
                '" fill="', face.color, '" opacity="', opacity, '" stroke="#000" stroke-width="0.5"/>'
            ));
        }

        svgOutput = string(abi.encodePacked(
            svgHeader,
            faces,
            '</g></svg>'
        ));

        return svgOutput;
    }

    /**
     * @dev Core Function 3: Update rendering parameters for dynamic 3D manipulation
     * @param tokenId ID of the 3D object
     * @param newParams New rendering parameters
     */
    function updateRenderParams(uint256 tokenId, RenderParams memory newParams) external {
        require(_exists(tokenId), "Token does not exist");
        require(
            ownerOf(tokenId) == msg.sender || owner() == msg.sender,
            "Not authorized to update render params"
        );

        _renderParams[tokenId] = newParams;
        emit RenderParamsUpdated(tokenId, msg.sender);
    }

    /**
     * @dev Project 3D coordinates to 2D screen coordinates
     */
    function project3DTo2D(Vertex3D memory vertex, RenderParams memory params) 
        internal 
        pure 
        returns (int256 x2d, int256 y2d) 
    {
        // Translate relative to camera
        int256 relativeX = vertex.x - params.cameraX;
        int256 relativeY = vertex.y - params.cameraY;
        int256 relativeZ = vertex.z - params.cameraZ;

        // Perspective projection
        if (relativeZ == 0) relativeZ = 1; // Avoid division by zero
        
        x2d = (relativeX * PROJECTION_DISTANCE) / (relativeZ + PROJECTION_DISTANCE);
        y2d = (relativeY * PROJECTION_DISTANCE) / (relativeZ + PROJECTION_DISTANCE);
    }

    /**
     * @dev Calculate basic lighting using face normal and light direction
     */
    function calculateLighting(
        Vertex3D memory v1, 
        Vertex3D memory v2, 
        Vertex3D memory v3,
        RenderParams memory params
    ) internal pure returns (int256) {
        // Calculate face normal (simplified)
        int256 nx = (v2.y - v1.y) * (v3.z - v1.z) - (v2.z - v1.z) * (v3.y - v1.y);
        int256 ny = (v2.z - v1.z) * (v3.x - v1.x) - (v2.x - v1.x) * (v3.z - v1.z);
        int256 nz = (v2.x - v1.x) * (v3.y - v1.y) - (v2.y - v1.y) * (v3.x - v1.x);

        // Light direction (normalized conceptually)
        int256 lightDot = nx * params.lightX + ny * params.lightY + nz * params.lightZ;
        
        return lightDot > 0 ? lightDot / 1000 : 0; // Simplified lighting calculation
    }

    /**
     * @dev Helper function to calculate opacity based on lighting
     */
    function _calculateOpacity(int256 lighting) internal pure returns (string memory) {
        if (lighting > 800) return "1.0";
        if (lighting > 600) return "0.8";
        if (lighting > 400) return "0.6";
        if (lighting > 200) return "0.4";
        return "0.2";
    }

    /**
     * @dev Convert int256 to string
     */
    function _int256ToString(int256 value) internal pure returns (string memory) {
        if (value == 0) return "0";
        
        bool negative = value < 0;
        if (negative) value = -value;
        
        uint256 temp = uint256(value);
        uint256 digits;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }
        
        bytes memory buffer = new bytes(digits + (negative ? 1 : 0));
        uint256 index = digits + (negative ? 1 : 0);
        temp = uint256(value);
        
        while (temp != 0) {
            index--;
            buffer[index] = bytes1(uint8(48 + temp % 10));
            temp /= 10;
        }
        
        if (negative) {
            buffer[0] = "-";
        }
        
        return string(buffer);
    }

    // View functions
    function getObject3D(uint256 tokenId) external view returns (Object3D memory) {
        require(_exists(tokenId), "Token does not exist");
        return _objects[tokenId];
    }

    function getRenderParams(uint256 tokenId) external view returns (RenderParams memory) {
        require(_exists(tokenId), "Token does not exist");
        return _renderParams[tokenId];
    }

    function getCreatorObjects(address creator) external view returns (uint256[] memory) {
        return _creatorObjects[creator];
    }

    function tokenURI(uint256 tokenId) public view override returns (string memory) {
        require(_exists(tokenId), "Token does not exist");
        
        Object3D memory obj = _objects[tokenId];
        string memory svg = this.render3DToSVG(tokenId);
        
        string memory json = Base64.encode(
            bytes(
                string(
                    abi.encodePacked(
                        '{"name": "', obj.name, '",',
                        '"description": "On-chain 3D rendered object from Shackled Engine",',
                        '"image": "data:image/svg+xml;base64,', Base64.encode(bytes(svg)), '",',
                        '"attributes": [',
                        '{"trait_type": "Creator", "value": "', Strings.toHexString(uint160(obj.creator), 20), '"},',
                        '{"trait_type": "Vertices", "value": ', Strings.toString(obj.vertices.length), '},',
                        '{"trait_type": "Faces", "value": ', Strings.toString(obj.faces.length), '},',
                        '{"trait_type": "Created", "value": ', Strings.toString(obj.timestamp), '}',
                        ']}'
                    )
                )
            )
        );
        
        return string(abi.encodePacked("data:application/json;base64,", json));
    }
}
        Object3D storage obj = _objects[tokenId];
        RenderParams storage params = _renderParams[tokenId];

        // Start SVG construction
        string memory svgHeader = string(abi.encodePacked(
            '<svg width="800" height="600" xmlns="http://www.w3.org/2000/svg">',
            '<rect width="100%" height="100%" fill="', params.backgroundColor, '"/>',
            '<g transform="translate(400,300)">'
        ));

        string memory faces = "";
        
        // Render each face
        for (uint256 i = 0; i < obj.faces.length; i++) {
            Face3D memory face = obj.faces[i];
            
            // Get 3D vertices
            Vertex3D memory v1 = obj.vertices[face.v1];
            Vertex3D memory v2 = obj.vertices[face.v2];
            Vertex3D memory v3 = obj.vertices[face.v3];

            // Project 3D to 2D coordinates
            (int256 x1, int256 y1) = project3DTo2D(v1, params);
            (int256 x2, int256 y2) = project3DTo2D(v2, params);
            (int256 x3, int256 y3) = project3DTo2D(v3, params);

            // Calculate lighting (simple dot product with light direction)
            int256 lighting = calculateLighting(v1, v2, v3, params);
            string memory opacity = _calculateOpacity(lighting);

            // Create triangle polygon
            faces = string(abi.encodePacked(
                faces,
                '<polygon points="',
                _int256ToString(x1), ',', _int256ToString(y1), ' ',
                _int256ToString(x2), ',', _int256ToString(y2), ' ',
                _int256ToString(x3), ',', _int256ToString(y3),
                '" fill="', face.color, '" opacity="', opacity, '" stroke="#000" stroke-width="0.5"/>'
            ));
        }

        svgOutput = string(abi.encodePacked(
            svgHeader,
            faces,
            '</g></svg>'
        ));

        return svgOutput;
    }

    /**
     * @dev Core Function 3: Update rendering parameters for dynamic 3D manipulation
     * @param tokenId ID of the 3D object
     * @param newParams New rendering parameters
     */
    function updateRenderParams(uint256 tokenId, RenderParams memory newParams) external {
        require(_exists(tokenId), "Token does not exist");
        require(
            ownerOf(tokenId) == msg.sender || owner() == msg.sender,
            "Not authorized to update render params"
        );

        _renderParams[tokenId] = newParams;
        emit RenderParamsUpdated(tokenId, msg.sender);
    }

    /**
     * @dev Project 3D coordinates to 2D screen coordinates
     */
    function project3DTo2D(Vertex3D memory vertex, RenderParams memory params) 
        internal 
        pure 
        returns (int256 x2d, int256 y2d) 
    {
        // Translate relative to camera
        int256 relativeX = vertex.x - params.cameraX;
        int256 relativeY = vertex.y - params.cameraY;
        int256 relativeZ = vertex.z - params.cameraZ;

        // Perspective projection
        if (relativeZ == 0) relativeZ = 1; // Avoid division by zero
        
        x2d = (relativeX * PROJECTION_DISTANCE) / (relativeZ + PROJECTION_DISTANCE);
        y2d = (relativeY * PROJECTION_DISTANCE) / (relativeZ + PROJECTION_DISTANCE);
    }

    /**
     * @dev Calculate basic lighting using face normal and light direction
     */
    function calculateLighting(
        Vertex3D memory v1, 
        Vertex3D memory v2, 
        Vertex3D memory v3,
        RenderParams memory params
    ) internal pure returns (int256) {
        // Calculate face normal (simplified)
        int256 nx = (v2.y - v1.y) * (v3.z - v1.z) - (v2.z - v1.z) * (v3.y - v1.y);
        int256 ny = (v2.z - v1.z) * (v3.x - v1.x) - (v2.x - v1.x) * (v3.z - v1.z);
        int256 nz = (v2.x - v1.x) * (v3.y - v1.y) - (v2.y - v1.y) * (v3.x - v1.x);

        // Light direction (normalized conceptually)
        int256 lightDot = nx * params.lightX + ny * params.lightY + nz * params.lightZ;
        
        return lightDot > 0 ? lightDot / 1000 : 0; // Simplified lighting calculation
    }

    /**
     * @dev Helper function to calculate opacity based on lighting
     */
    function _calculateOpacity(int256 lighting) internal pure returns (string memory) {
        if (lighting > 800) return "1.0";
        if (lighting > 600) return "0.8";
        if (lighting > 400) return "0.6";
        if (lighting > 200) return "0.4";
        return "0.2";
    }

    /**
     * @dev Convert int256 to string
     */
    function _int256ToString(int256 value) internal pure returns (string memory) {
        if (value == 0) return "0";
        
        bool negative = value < 0;
        if (negative) value = -value;
        
        uint256 temp = uint256(value);
        uint256 digits;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }
        
        bytes memory buffer = new bytes(digits + (negative ? 1 : 0));
        uint256 index = digits + (negative ? 1 : 0);
        temp = uint256(value);
        
        while (temp != 0) {
            index--;
            buffer[index] = bytes1(uint8(48 + temp % 10));
            temp /= 10;
        }
        
        if (negative) {
            buffer[0] = "-";
        }
        
        return string(buffer);
    }

    // View functions
    function getObject3D(uint256 tokenId) external view returns (Object3D memory) {
        require(_exists(tokenId), "Token does not exist");
        return _objects[tokenId];
    }

    function getRenderParams(uint256 tokenId) external view returns (RenderParams memory) {
        require(_exists(tokenId), "Token does not exist");
        return _renderParams[tokenId];
    }

    function getCreatorObjects(address creator) external view returns (uint256[] memory) {
        return _creatorObjects[creator];
    }

    function tokenURI(uint256 tokenId) public view override returns (string memory) {
        require(_exists(tokenId), "Token does not exist");
        
        Object3D memory obj = _objects[tokenId];
        string memory svg = this.render3DToSVG(tokenId);
        
        string memory json = Base64.encode(
            bytes(
                string(
                    abi.encodePacked(
                        '{"name": "', obj.name, '",',
                        '"description": "On-chain 3D rendered object from Shackled Engine",',
                        '"image": "data:image/svg+xml;base64,', Base64.encode(bytes(svg)), '",',
                        '"attributes": [',
                        '{"trait_type": "Creator", "value": "', Strings.toHexString(uint160(obj.creator), 20), '"},',
                        '{"trait_type": "Vertices", "value": ', Strings.toString(obj.vertices.length), '},',
                        '{"trait_type": "Faces", "value": ', Strings.toString(obj.faces.length), '},',
                        '{"trait_type": "Created", "value": ', Strings.toString(obj.timestamp), '}',
                        ']}'
                    )
                )
            )
        );
        
        return string(abi.encodePacked("data:application/json;base64,", json));
    }
}
