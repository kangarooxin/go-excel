# go-excel
- depend on [excelize](https://github.com/qax-os/excelize)
- read and write excel with struct

## Functions
- read from filepath `GetRowsFromFile`
- read from io.Read `GetRowsFromRead`
- read from upload multipart file `GetRowsFromMultipart`

## Usage
1. import lib
   ```go
   go get -u github.com/kangarooxin/go-excel
   ```

2. mark struct column with tag `xlsx`, if not config tag, use field name default.

    ```go
    type User struct {
    	Id       int       `xlsx:"账号ID"`
    	Name     string    `xlsx:"账号名"`
    	Birthday time.Time `xlsx:"生日"`
        Interest []string  `xlsx:"兴趣"`
	    Numbers  []int     `xlsx:"数字"`
    }
    ```

3. create excel with struct slice

    ```go
    func TestCreate(t *testing.T) {
    	users := &[]User{
    		{
    			Id:       1,
    			Name:     "Test1",
    			Birthday: time.Now(),
                Interest: []string{"篮球", "户外"},
			    Numbers:  []int{1, 2},
    		},
    		{
    			Id:       2,
    			Name:     "Test2",
    			Birthday: time.Now(),
                Interest: []string{"篮球", "户外"},
			    Numbers:  []int{1, 2},
    		},
    	}
    	f, err := excel.NewFile(users)
    	if err != nil {
    		fmt.Println(err)
    		return
    	}
    	if err := f.SaveAs("Test1.xlsx"); err != nil {
    		fmt.Println(err)
    	}
    }
    ```

4. read excel to struct slice

    ```go
    func TestRead(t *testing.T) {
    	users := &[]User{}
    	err = excel.GetRowsFromFile("Test1.xlsx", users)
    	if err != nil {
    		fmt.Println(err)
    		return
    	}
    	fmt.Println(users)
    }
    ```