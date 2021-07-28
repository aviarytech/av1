# av1

interfaces for libraries https://hackmd.io/0EHQ6RWfTnqzO5C8rzOmug

@aviarytech/dids
```typescript=
/**
 * A verification method definition entry in a DID Document.
 */
interface IDIDDocumentVerificationMethod {
  /** Fully qualified identifier of this public key, e.g. did:example:123#key-1 */
  id: string;
  
  /** The type of this public key, as defined in: https://w3c-ccg.github.io/ld-cryptosuite-registry/ */
  type: string;
  
  /** The DID of the controller of this key. */
  controller: string;
  
  /** The value of the public key in PEM format. Only one value field will be present. */
  publicKeyPem?: string;
  
  /** The value of the public key in JWK format. Only one value field will be present. */
  publicKeyJwk?: any;
  
  /** The value of the public key in hex format. Only one value field will be present. */
  publicKeyHex?: string;
  
  /** The value of the public key in Base64 format. Only one value field will be present. */
  publicKeyBase64?: string;
  
  /** The value of the public key in Base58 format. Only one value field will be present. */
  publicKeyBase58?: string;
  
  /** The value of the public key in Multibase format. Only one value field will be present. */
  publicKeyMultibase?: string;
  
  /** Returns the public key in JWK format regardless of the data type */
  asJwk(): JWK;
}

/**
 * Defines a service descriptor entry present in a DID Document.
 */
interface IDIDDocumentServiceDescriptor {
  /** id of this service, e.g. `did:example:123#id`. */
  id: string;
  
  /** The type of this service. */
  type: string;
  
  /** The endpoint of this service, as a URI. */
  serviceEndpoint: string;
  
  /** didcomm service extension */
  routingKeys: string[];
}

/**
 * Decentralized Identity Document.
 */
interface IDIDDocument {
  /** The JSON-LD context of the DID Documents. */
  context: string[];
  
  /** The DID to which this DID Document pertains. */
  id: string;
  
  /** Array of verification methods associated with the DID. */
  verificationMethod?: IDIDDocumentVerificationMethod[];
  
  /** Array of services associated with the DID. */
  service?: IDIDDocumentServiceDescriptor[];
  
  /** Array of authentication methods. */
  authentication?: IDIDDocumentVerificationMethod[];
  
  /** Array of assertion methods. */
  assertionMethod?: IDIDDocumentVerificationMethod[];
  
  /** Array of key agreement methods */
  keyAgreement?: IDIDDocumentVerificationMethod[];
}

interface IDIDResolver {
  resolve(id: string): Promise<IDIDDocument>;
}
```

@aviarytech/didcomm
```typescript=
const enum DIDCommMessageMediaType {
  PLAIN = "application/didcomm-plain+json",
  SIGNED = "application/didcomm-signed+json",
  ENCRYPTED = "application/didcomm-encrypted+json",
}

interface RecipientHeader {
  typ: string;
  alg: string;
  kid: string;
}

interface JWE {
  protected: string
  iv: string
  ciphertext: string
  tag: string
  aad?: string
  recipients?: RecipientHeader[]
}

interface JWS {
  header: RecipientHeader;
  payload: string;
  signature: string;
  protected?: string;
}

interface JWK {
  alg?: string
  crv?: string
  d?: string
  dp?: string
  dq?: string
  e?: string
  ext?: boolean
  k?: string
  key_ops?: string[]
  kid?: string
  kty?: string
  n?: string
  oth?: Array<{
    d?: string
    r?: string
    t?: string
  }>
  p?: string
  q?: string
  qi?: string
  use?: string
  x?: string
  y?: string
  x5c?: string[]
  x5t?: string
  'x5t#S256'?: string
  x5u?: string
  [propName: string]: unknown
}

interface IDIDCommAttachment {
  id: string;
  description?: string;
  filename?: string;
  mime_type?: string;
  lastmod_time?: string;
  byte_count?: number;
  data: {
    jws?: JWS;
    hash?: string;
    links?: string[];
    base64?: string;
    jwe?: JWE;
    json?: object;
  };
}

interface IDIDCommPayload {
  id: string;
  type: string;
  from?: string;
  to: string;
  thid?: string;
  pthid?: string;
  expires_time?: string;
  created_time?: string;
  next?: string;
  from_prior?: string;
  body: any;
  attachments?: IDIDCommAttachment[];
}

interface IDIDCommMessage {
  payload: IDIDCommPayload;
  repudiable: boolean;
  headers?: IDIDCommHeader[];
  signature?: string;
}

interface IDIDCommCore {}
```

@aviarytech/secrets
```typescript==
interface DIDSecret {
  /** Fully qualified identifier of the public key to this private key, e.g. did:example:123#key-1 */
  id: string;
  
  /** The type of this private key, as defined in: https://w3c-ccg.github.io/ld-cryptosuite-registry/ */
  type: string;
  
  /** The DID of the controller of this key. */
  controller: string;
  
  /** The value of the private key in PEM format. Only one value field will be present. */
  privateKeyPem?: string;
  
  /** The value of the private key in JWK format. Only one value field will be present. */
  privateKeyJwk?: any;
  
  /** The value of the private key in hex format. Only one value field will be present. */
  privateKeyHex?: string;
  
  /** The value of the private key in Base64 format. Only one value field will be present. */
  privateKeyBase64?: string;
  
  /** The value of the private key in Base58 format. Only one value field will be present. */
  privateKeyBase58?: string;
  
  /** The value of the private key in Multibase format. Only one value field will be present. */
  privateKeyMultibase?: string;
  
  /** Returns the private key in JWK format regardless of the data type */
  asJwk(): JWK;
}

interface DIDSecretResolver {
  resolve(id: string): Promise<DIDSecret>;
}
```
