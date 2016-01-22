# iOS
#数据结构
##线性表
0或多个数据元素组成的有限序列

顺序存储结构：用一段地址连续的存储单元一次存储线性表的数据元素（数组）

链式存储结构：

```
typedef struct Node {
	ElemType data; //数据域
	struct Node * Next; //指针域
} Node;
```
线性表初始化：

```
-(SqList *)initSqList {
    sqList = (SqList *)malloc(sizeof(SqList)); //分配空间
    sqList->length = 0; //初始表长为0
    return sqList; //返回线性表指针
}
```
线性表表尾加入新元素：

```
-(Status)putElement:(ElemType)element intoSqList:(SqList *)list {
    int length = list->length;
    if (length < 0 || length >= MAXSIZE) { //如果长度小于零，或大于最大长度，返回1表示异常
        return 1;
    }
    list->data[length] = element; //线性表的最后一位的元素为插入的元素
    list->length++; //因为插入了一个元素，表长加一
    return 0; //返回0表示正常
}
```

遍历线性表所有元素：


```
-(void)outputElementOnList:(SqList *)list {
    for (int i = 0; i < list->length; i++) {
        NSLog(@"%d",list->data[i]);
    }
    NSLog(@"线性表表长：%d",list->length);
}

```

线性表插入：

```
-(Status)insertElement:(ElemType)element intoSqList:(SqList *)list onIndex:(int)index {
    int length = list->length;
    
    if (length == MAXSIZE) {
        return -1;
    }
    if (index < 1 || index > length + 1) {
        return -1;
    }
    //正常插入
    if (index <= length) {
        for (int i = length - 1; i >= index -1; i--) {
            list->data[i+1] = list->data[i]; //数组中第index个元素及后面的元素都要向后移一位。
        }
    }
    list->data[index-1] = element;
    list->length++;
    return 0;
}
```

线性表删除：

```
-(Status)deleteElementOnIndex:(int)index whichSqList:(SqList *)list {
    if (index > list->length || index < 1) {
        return -1;
    }
    
    ElemType e = list->data[index-1];
    NSLog(@"删除的是第%d个元素%d",index,e);
    
    if (index < list->length) {
        for (int i = index; i < list->length; i++) {//index-1开始的位置向前移动一位
            list->data[i-1] = list->data[i];
        }
    }
    
    list->length--;
    return 0;
}

```
线性表查找第index位元素：

```
-(Status)getElementOnIndex:(int)index whichSqList:(SqList *)list {
    if (index > list->length || index < 1) {
        return -1;
    }
    ElemType e = list->data[index-1];
    NSLog(@"第%d个元素是%d",index,e);
    return 0;

}
```
##链表

建立链表：

```
-(void)createDLB{
    head = [[DLB alloc]init]; //初始化一个头
    head->next = nil;
    
    DLB *temp = head;
    for (int i = 1; i <= 20; i++) {
        DLB *node = [[DLB alloc]init];
        node->data = [NSNumber numberWithInt:i];
        node->next = nil;
        temp->next = node;
        temp = node;
    }
}
```

插入元素到链表：

```
-(void)insert:(int)value intoDLBatIndex:(int)index {
    DLB *temp = head; //获取头节点
    for (int i = 1; i < index; i++) {
        temp = temp->next;//头节点后移到要插入到节点的前一个节点
    }
    DLB *insertNode = [[DLB alloc]init];
    insertNode->data = [NSNumber numberWithInt:value];
    insertNode->next = temp->next; //插入的节点的后继节点是index位置前的元素的后继节点
    temp->next = insertNode;
}
```
删除链表元素：

```
-(void)deleteDLBatIndex:(int)index {
    DLB *temp = head;
    for (int i = 1; i < index; i++) {
        temp = temp->next;
    }
    DLB *deleteNode = temp->next;
    temp->next = deleteNode->next;//让指针跳过要删除的
}

```

查找第index个位置的链表元素：

```
-(id)getNodeAtIndex:(int)index {
    DLB *temp = head;
    DLB *returnNode;
    for (int i = 1; i < index; i++) {
        temp = temp->next;
    }
    returnNode = temp;
    return returnNode->data;
}
```

遍历链表：

```
-(void)outputDLB {
    DLB *temp = head->next;
    while (temp) {
        NSLog(@"%d",[temp->data intValue]);
        temp = temp->next;
    }
}
```

##二叉树

```
@interface BTree : NSObject

@property (nonatomic,strong) BTreeNode *root;

-(instancetype)initWithData:(NSObject *)data;

- (void)preOrderTraversal;
- (void)inOrderTraversal;
- (void)postOrderTraversal;

@end
```


```
@interface BTreeNode : NSObject

@property (nonatomic,strong) NSObject  *data;
@property (nonatomic,strong) BTreeNode *parent;
@property (nonatomic,strong) BTreeNode *leftChild;
@property (nonatomic,strong) BTreeNode *rightChild;
@end
```

```
-(instancetype)initWithData:(NSObject *)data {
    self = [super init];
    if (self) {
        _root = [[BTreeNode alloc]init];
        self.root.data = data;
    }
    return self;
}

-(BOOL)insertNode:(BTreeNode *)node parent:(BTreeNode *)parent isLeftChild:(BOOL)isLeft {
    if (isLeft && parent.leftChild == nil) {
        parent.leftChild = node;
    } else if (parent.rightChild == nil) {
        parent.rightChild = node;
   }
    return true;
}

- (void)preOrderTraversal {
    if (self.root) {
        [BTree preOrderTraversalRecursive:self.root];
    }
}

- (void)inOrderTraversal {
    if (self.root) {
        [BTree inOrderTraversalRecursive:self.root];
    }
}

- (void)postOrderTraversal {
    if (self.root) {
        [BTree postOrderTraversalRecursive:self.root];
    }
}

+ (void)preOrderTraversalRecursive:(BTreeNode *)node {
    if (node) {
        NSLog(@"%@",node.data);
        [BTree preOrderTraversalRecursive:node.leftChild];
        [BTree preOrderTraversalRecursive:node.rightChild];
    }
}

+ (void)inOrderTraversalRecursive:(BTreeNode *)node {
    if (node) {
        [BTree inOrderTraversalRecursive:node.leftChild];
        NSLog(@"%@",node.data);
        [BTree inOrderTraversalRecursive:node.rightChild];
    }
}

+ (void)postOrderTraversalRecursive:(BTreeNode *)node {
    if (node) {
        [BTree postOrderTraversalRecursive:node.leftChild];
        [BTree postOrderTraversalRecursive:node.rightChild];
        NSLog(@"%@",node.data);
    }
}
```
#iOS

##文件保存－固化

```
- (void)encodeWithCoder:(NSCoder *)aCoder {
    [aCoder encodeObject:self.name forKey:@"name"];
    [aCoder encodeObject:self.age forKey:@"age"];
}

- (instancetype)initWithCoder:(NSCoder *)aDecoder {
    self = [super init];
    if (self) {
        self.name = [aDecoder decodeObjectForKey:@"name"];
        self.age = [aDecoder decodeObjectForKey:@"age"];
    }
    return self;
}

-(NSString *)documentPath {
    NSString *path = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject];
    return [path stringByAppendingPathComponent:@"item.archive"];
}

-(BOOL)save {
    NSString *path = [self documentPath];
    return [NSKeyedArchiver archiveRootObject:self.name toFile:path];
}

-(instancetype)initPrivate {
    self = [super init];
    if (self) {
        NSString *path = [self documentPath];
        self.name = [NSKeyedUnarchiver unarchiveObjectWithFile:path];
    }
    return self;
}

```

##coredata

```
使用Core Data保存数据

首先需要有一个NSManagedObjectContext对象
得到NSManagedObjectContext的方法有很多

1，使用UIManagedDocument
UIManagedDocument有一个managedObjectContext的属性

一般方法：
首先使用NSFileManager获取沙盒中的路径。

-(UIManagedDocument *)createManagedDocument
{
    NSFileManager *fm = [NSFileManager defaultManager];
    NSURL *documentsDirectory = [[fm URLsForDirectory:NSDocumentDirectory inDomains:NSUserDomainMask] firstObject];
    //URLsForDirectory: inDomains: 返回值是一个包含NSURL的NSArray对象，与下面的C函数有所不同，需要注意
    /*
     或者使用以下方法：
     
     NSArray *documentDirectories = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
     NSString *documentDirectory  = [documentDirectories firstObject];
     
     NSSearchPathForDirectoriesInDomains是C函数，该函数有三个参数，其中后两个实参需要传入固定值（该函数源于OS X,在iOS上只有唯一值），第一个参数是NSSearchPathDirectory类型的常量，负责指定目录的类型，例如，传入NSCacheDirectory可以得到沙盒中的Caches目录的路径，此函数的返回值是一个NSArray对象，包含的都是NSString。
     */
    
    NSString *document = @"myDocument";
    NSURL *url = [documentsDirectory URLByAppendingPathComponent:document];
    
    UIManagedDocument *managedDocument = [[UIManagedDocument alloc]initWithFileURL:url];
    
    //打开或者创建一个UIManagedDocument之前需要先检查文件是否存在
    BOOL fileExists = [[NSFileManager defaultManager] fileExistsAtPath:[url path]];
    
    if (fileExists) {
        //存在，则打开，进行操作
        [managedDocument openWithCompletionHandler:^(BOOL success) {
            if (success) {
                [self documentIsReady];
            }else
                NSLog(@"couldn't open document at %@",url);
            
        }];
    }else{
        //不存在，则先在此路径创建一个文件再进行操作
        [managedDocument saveToURL:url forSaveOperation:UIDocumentSaveForCreating completionHandler:^(BOOL success) {
            if (success) {
                [self documentIsReady];
            }else
                NSLog(@"couldn't create document at %@",url);
        }];
    }
    return managedDocument;
    
}

-(void)documentIsReady
{
    //在获取UIManagedDocument的context之前需要先检查documentState是否是UIDocumentStateNormal
    if (self.managedDocument.documentState == UIDocumentStateNormal) {
        //现在可以开始做一些core data的事情
        NSManagedObjectContext *context = self.managedDocument.managedObjectContext;
        
        //插入（新增）一个对象到数据库中：
        NSManagedObject *photo = [NSEntityDescription insertNewObjectForEntityForName:@"Photo" //此处填插入的对象是属于哪个类型的实体
                                                               inManagedObjectContext:context];
        
        //在数据模型中选择Xcode的editor-- create NSManagedObject Subclass..可以创建数据模型的子类，从而进行Objective－C风格的访问属性和修改属性的操作。
        
        //删除
        
        [self.managedDocument.managedObjectContext deleteObject:photo];
        
        //查找
        NSString *photographer = @"陈冠希";
        NSFetchRequest *request = [NSFetchRequest fetchRequestWithEntityName:@"Photo"]; //查找要有一个fetchRequest
        request.predicate = [NSPredicate predicateWithFormat:@"whoTook = %@",photographer];//fetchRequest的谓词（按条件过滤，nil表示返回全部结果）
        request.sortDescriptors = @[[NSSortDescriptor sortDescriptorWithKey:@"date" ascending:YES]];//request的排序方法
        
        NSArray *results = [context executeFetchRequest:request error:nil];//返回的是一个数组
        
    }
    
}

2，创建一个NSManagedObjectContext

NSManagedObjectContext负责应用和数据库之间的交互工作，通过NSManagedObjectContext对象所使用的NSPersistentStoreCoordinator对象，可以指定文件路径并打开相应的SQLite数据库，NSPersistentStoreCoordinator需要配合某个模型文件才能工作（NSManagedObjectModel对象可以代表模型文件）。

-(void)anotherWayToCreateNSManagedObjectContext
{
    //首先需要有一个NSManagedObjectModel模型
    _model = [NSManagedObjectModel mergedModelFromBundles:nil];
    
    //使用上面的NSManagedObjectModel模型初始化一个NSPersistentStoreCoordinator对象
    NSPersistentStoreCoordinator *psc = [[NSPersistentStoreCoordinator alloc] initWithManagedObjectModel:_model];
    
    // Where does the SQLite file go?
    
    NSArray *documentDirectories = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
    
    NSString *documentDirectory  = [documentDirectories firstObject];
    
    NSString *path = [documentDirectory stringByAppendingPathComponent:@"store.data"];
    
    NSURL *storeURL = [NSURL fileURLWithPath:path];
    
    NSError *error = nil;
    
    
    
    if (![psc addPersistentStoreWithType:NSSQLiteStoreType
                           configuration:nil
                                     URL:storeURL
                                 options:nil
                                   error:&error]) {
        @throw [NSException exceptionWithName:@"OpenFailure"
                                       reason:[error localizedDescription]
                                     userInfo:nil];
    }
    
    // Create the managed object context
    _context = [[NSManagedObjectContext alloc] init];
    _context.persistentStoreCoordinator = psc;
    
}




``` 
