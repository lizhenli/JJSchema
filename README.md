JJSchema
===============

A Java JSON Schema and Hyper-Schema generator.
Currently, it is based on v4 draft.

Lastest Release
----------------

    <dependency>
      <groupId>com.github.reinert</groupId>
      <artifactId>jjschema</artifactId>
      <version>0.6</version>
    </dependency>

Simple HOW TO
----------------

Suppose the following Class:

    @Attributes(title="Product", description="A product from Acme's catalog")
    static class Product {
    	@Attributes(required=true, description="The unique identifier for a product")
    	private long id;
    	@Attributes(required=true, description="Name of the product")
    	private String name;
    	@Attributes(required=true, minimum=0, exclusiveMinimum=true)
    	private BigDecimal price;
    	@Attributes(minItems=1,uniqueItems=true)
    	private List<String> tags;
    	
    	public long getId() {
    		return id;
    	}
    	public void setId(long id) {
    		this.id = id;
    	}
    	public String getName() {
    		return name;
    	}	
    	public void setName(String name) {
    		this.name = name;
    	}
    	public BigDecimal getPrice() {
    		return price;
    	}
    	public void setPrice(BigDecimal price) {
    		this.price = price;
    	}
    	public List<String> getTags() {
    		return tags;
    	}
    	public void setTags(List<String> tags) {
    		this.tags = tags;
    	}
    }

Type the following code:

    JsonSchemaGenerator v4generator = SchemaGeneratorBuilder.draftV4Schema().build();
    JsonNode productSchema = v4generator.generateSchema(Product.class);
    System.out.println(productSchema);


The output:

    {
        "$schema":"http://json-schema.org/draft-04/schema#",
        "type":"object",
        "title":"Product",
        "description":"A product from Acme's catalog",
        "required":["price","name","id"],
        "properties":{
            "id":{
                "type":"integer",
                "description":"The unique identifier for a product"
            },
            "tags":{
                "type":"array",
                "items":{"type":"string"},
                "minItems":1,
                "uniqueItems":true
            },
            "price":{
                "type":"number",
                "minimum":0,
                "exclusiveMinimum":true
            },
            "name":{
                "type":"string",
                "description":"Name of the product"
            }
        }
    }
