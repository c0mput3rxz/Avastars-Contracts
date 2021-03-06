# Avastar Trait Factory

View Source: [contracts/TraitFactory.sol](https://github.com/Dapp-Wizards/Avastars-Contracts/blob/master/contracts/TraitFactory.sol)

**TraitFactory** **↗ Extends: [AvastarState](contracts/AvastarState.md)**
**↘ Derived Contracts: [AvastarFactory](contracts/AvastarFactory.md), [TraitFactoryWrapper](contracts/TraitFactoryWrapper.md)**

## **Events**

- [NewTrait](#newtrait)
- [AttributionSet](#attributionset)
- [TraitArtExtended](#traitartextended)

### NewTrait

Event emitted when a new Trait is created.

```solidity
event NewTrait(
	uint256 id,
	enum AvastarTypes.Generation generation,
	enum AvastarTypes.Gene gene,
	enum AvastarTypes.Rarity rarity,
	uint8 variation,
	string name
)
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| id | uint256 | the Trait ID | 
| generation | enum AvastarTypes.Generation | the generation of the trait | 
| gene | enum AvastarTypes.Gene | ration the generation of the trait | 
| rarity | enum AvastarTypes.Rarity | the rarity level of this trait | 
| variation | uint8 | variation of the gene the trait represents | 
| name | string | the name of the trait | 

### AttributionSet

Event emitted when artist attribution is set for a generation.

```solidity
event AttributionSet(
	enum AvastarTypes.Generation generation,
	string artist,
	string infoURI
)
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| generation | enum AvastarTypes.Generation | the generation that attribution was set for | 
| artist | string | the artist who created the artwork for the generation | 
| infoURI | string | the artist's website / portfolio URI | 

### TraitArtExtended

Event emitted when a Trait's art is created.

```solidity
event TraitArtExtended(uint256 id)
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| id | uint256 | the Trait ID | 

## Modifiers

- [onlyBeforeProd](#onlybeforeprod)

### onlyBeforeProd

Modifier to ensure no trait modification after a generation's
Avastar production has begun.

```solidity
modifier onlyBeforeProd(enum AvastarTypes.Generation _generation) internal
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| _generation | enum AvastarTypes.Generation | the generation to check production status of | 

## **Functions**

- [getTraitIdByGenerationGeneAndVariation](#gettraitidbygenerationgeneandvariation)
- [getTraitInfoById](#gettraitinfobyid)
- [getTraitNameById](#gettraitnamebyid)
- [getTraitArtById](#gettraitartbyid)
- [getAttributionByGeneration](#getattributionbygeneration)
- [setAttribution](#setattribution)
- [createTrait](#createtrait)
- [extendTraitArt](#extendtraitart)
- [assembleArtwork](#assembleartwork)

### getTraitIdByGenerationGeneAndVariation

Get Trait ID by Generation, Gene, and Variation.

```solidity
function getTraitIdByGenerationGeneAndVariation(
	enum AvastarTypes.Generation _generation,
	enum AvastarTypes.Gene _gene,
	uint8 _variation
)
external view
returns (uint256 traitId)
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| _generation | enum AvastarTypes.Generation | the generation the trait belongs to | 
| _gene | enum AvastarTypes.Gene | ration the generation the trait belongs to | 
| _variation | uint8 | the variation of the gene | 

**Returns**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| traitId | uint256 | the ID of the specified trait | 

### getTraitInfoById

Retrieve a Trait's info by ID.

```solidity
function getTraitInfoById(uint256 _traitId)
external view
returns (
	uint256 id,
	enum AvastarTypes.Generation generation,
	enum AvastarTypes.Series[] series,
	enum AvastarTypes.Gender gender,
	enum AvastarTypes.Gene gene,
	enum AvastarTypes.Rarity rarity,
	uint8 variation,
	string name
)
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| _traitId | uint256 | the ID of the Trait to retrieve | 

**Returns**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| id | uint256 | the ID of the trait | 
| generation | enum AvastarTypes.Generation | generation of the trait | 
| series | enum AvastarTypes.Series[] | list of series the trait may appear in | 
| gender | enum AvastarTypes.Gender | gender(s) the trait is valid for | 
| gene | enum AvastarTypes.Gene | ration generation of the trait | 
| rarity | enum AvastarTypes.Rarity | the rarity level of this trait | 
| variation | uint8 | variation of the gene the trait represents | 
| name | string | name of the trait | 

### getTraitNameById

Retrieve a Trait's name by ID.

```solidity
function getTraitNameById(uint256 _traitId)
external view
returns (string name)
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| _traitId | uint256 | the ID of the Trait to retrieve | 

**Returns**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| name | string | name of the trait | 

### getTraitArtById

Retrieve a Trait's art by ID.
Only invokable by a system administrator.

```solidity
function getTraitArtById(uint256 _traitId)
external view onlySysAdmin 
returns (string art)
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| _traitId | uint256 | the ID of the Trait to retrieve | 

**Returns**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| art | string | the svg layer representation of the trait | 

### getAttributionByGeneration

Get the artist Attribution info for a given Generation, combined into a single string.

```solidity
function getAttributionByGeneration(enum AvastarTypes.Generation _generation)
external view
returns (string attribution)
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| _generation | enum AvastarTypes.Generation | the generation to retrieve artist attribution for | 

**Returns**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| attribution | string | attrib a single string with the artist and artist info URI | 

### setAttribution

Set the artist Attribution for a given Generation

```solidity
function setAttribution(
	enum AvastarTypes.Generation _generation,
	string _artist,
	string _infoURI
)
external nonpayable onlySysAdmin onlyBeforeProd 
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| _generation | enum AvastarTypes.Generation | the generation to set artist attribution for | 
| _artist | string | the artist who created the art for the generation | 
| _infoURI | string | the URI for the artist's website / portfolio | 

### createTrait

Create a Trait

```solidity
function createTrait(
	enum AvastarTypes.Generation _generation,
	enum AvastarTypes.Series[] _series,
	enum AvastarTypes.Gender _gender,
	enum AvastarTypes.Gene _gene,
	enum AvastarTypes.Rarity _rarity,
	uint8 _variation,
	string _name,
	string _svg
)
external nonpayable onlySysAdmin whenNotPaused onlyBeforeProd 
returns (uint256 traitId)
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| _generation | enum AvastarTypes.Generation | the generation the trait belongs to | 
| _series | enum AvastarTypes.Series[] | list of series the trait may appear in | 
| _gender | enum AvastarTypes.Gender | gender the trait is valid for | 
| _gene | enum AvastarTypes.Gene | ration the generation the trait belongs to | 
| _rarity | enum AvastarTypes.Rarity | the rarity level of this trait | 
| _variation | uint8 | the variation of the gene the trait belongs to | 
| _name | string | the name of the trait | 
| _svg | string | svg layer representation of the trait | 

**Returns**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| traitId | uint256 | the token ID of the newly created trait | 

### extendTraitArt

Extend a Trait's art.
Only invokable by a system administrator.
If successful, emits a `TraitArtExtended` event with the resultant artwork.

```solidity
function extendTraitArt(uint256 _traitId, string _svg)
external nonpayable onlySysAdmin whenNotPaused onlyBeforeProd 
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| _traitId | uint256 | the ID of the Trait to retrieve | 
| _svg | string | the svg content to be concatenated to the existing svg property | 

### assembleArtwork

Assemble the artwork for a given Trait hash with art from the given Generation

```solidity
function assembleArtwork(enum AvastarTypes.Generation _generation, uint256 _traitHash)
internal view
returns (string svg)
```

**Arguments**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| _generation | enum AvastarTypes.Generation | the generation the Avastar belongs to | 
| _traitHash | uint256 | the Avastar's trait hash | 

**Returns**

| Name        | Type           | Description  |
| ------------- |------------- | -----|
| svg | string | the fully rendered SVG for the Avastar | 

