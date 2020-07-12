# Dotnet-GraphQL.Code.Generator
Generate GraphQL classes from .net Domain Classes.


///////////////////////////////////////  Pagination and Search Example //////////////////////////////////////////////////////////////////////

"Query" : query($pagination: PaginationInputType, $search: SearchInputType) {order(pagination: $pagination, search: $search) {id, orderDate quantity customerId}}
,"Variables" :{ "pagination" : {"take": 100, "skip": 100, "sorts": [{"fieldName": "created", "direction": "desc"}, {"fieldName": "quantity", "direction": "asc"}]}, "search" : {"filters": [{ "logic": "or", "operation": "eq", "value": 3245, "fieldName": "orderId" }, { "logic": "and", "operation": "inlist", "value": "32,66,87,199", "fieldName": "customerId" }, { "logic": "and", "operation": "gte", "value": 600, "fieldName": "customerId" } ]}}

///////////////////////////////////// List of Operators //////////////////////////////////////////////////////////////////////////////////////////

gte, gt, eq, neq, lte, lt, contains, notcontains, startswith, endswith, inlist, notinlist


///////////////////////////////////////Configuration Section////////////////////////////////////////////////////////////////////////////////////
Configuration.TypeClasses.RepositoryConstructorParameter
= "RepositoryNamespace.RepositoryClass repositoryConstructorParameterName";
Configuration.TypeClasses.RepositoryPrivateMember
= "RepositoryNamespace.RepositoryClass repositoryPrivateMemberName";
// In constructor of Type class it will translate as this.repositoryPrivateMemberName = repositoryConstructorParameterName;

Configuration.TypeClasses.TypeClassesNamespace = "GraphQL.Types.Custom";
// This will translate as namespace GraphQL.Types.Custom {...

Configuration.TypeClasses.EntityClassesNamespacesInclude = new Regex(@"^EntitiesNamespace$", RegexOptions.IgnoreCase);
// Any namespace in input dll (where entities classes are) match above regular expression will be included.

Configuration.TypeClasses.EntityClassNamesExclude = 
    new Regex(@"^Rusp.*$|^DatabaseInitializer$|^DBManager$|^DataSeeder$", RegexOptions.IgnoreCase);
// Any class match above regular expression will be excluded 

Configuration.TypeClasses.AdditionalNamespaces = "MyNamespace1, MyNamespace2";

string path = @"c:\dllFolder\EntitiesAssembly.dll";

Configuration.InputDllNameAndPath = path;
Configuration.TypeClasses.EntityClassNamesInclude
    = new Regex(@"^DomainClass1$|^DomainClass2$", RegexOptions.IgnoreCase);
// Any class in input dll (where entities classes are) match above regular expression will be included.

Dictionary<string, string> pluralizationFilter = new Dictionary<string, string>();
pluralizationFilter.Add("Person", "Persons");
Configuration.PluralizationFilter = pluralizationFilter;

Configuration.TypeClasses.AdditionalCodeToBeAddedInConstructor = "InitializePartial();";
//Above setting will create InitializePartial(); in the Type Classes constructor and this can be used to call partial class method. If you don't have InitializePartial in your 
// partial class or you don't have partial class at all then remove above line. But you can add this later if in all the classes you called some partial class method.

Configuration.RepositoryClass.DBContextPrivateMember
    = "MyDbContextNamespace.MyDbContextclass _context";
Configuration.RepositoryClass.DBContextConstructorParameter
    = "MyDbContextNamespace.MyDbContextclass context";
Configuration.RepositoryClass.RepositoryClassName = "CustomRepository";
Configuration.RepositoryClass.RepositoryClassesNamespace = "Custom.Repository.namespace";
Configuration.RepositoryClass.AdditionalNamespaces = "MyNamespace1, MyNamespace2";
//Configuration.RepositoryClass.MethodExcludeFilter = new List<string> { "GetPersons" };
//Configuration.RepositoryClass.IsMethodExcludeFilterApplyToInterface = true;

Configuration.QueryClass.RepositoryConstructorParameter
 = "RepositoryNamespace.RepositoryClass repositoryConstructorParameterName";
Configuration.QueryClass.RepositoryPrivateMember
Configuration.QueryClass.QueryClassName = "CustomGraphQLQuery";
Configuration.QueryClass.QueryClassNamespace = "Graph.Queries";
Configuration.QueryClass.AdditionalNamespaces = "MyNamespace1, MyNamespace2";

int fileCount = GraphQL.Code.Generator.GraphQLCodeGenerator.GenerateGraphQLCode();

