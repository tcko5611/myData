# Question
How to use a singleton? Howt to implement it?
# Answer
## Use of singleton
1. When we need a place to store the same data(like config file...) for different places to use it.
2. Make sure some operation will be use in a control center(like licens)
## Impletation
```cpp
class DataSingleton {
  public:
    static DataSingleton& Instance() {
      static DataSingleton inst;
      return inst;
    }
    map<int, vector<pair<string, int> > > netOpenMap() const;
    void setNetOpenMap(const Document &flistd, const afdata &afdata_obj);
  private:
    DataSingleton() = default; // anounce for private
    ~DataSingleton() = default;
    DataSingleton( const DataSingleton&) = delete; // avoid copy construtor
    DataSingleton& operator=(const DataSingleton&) = delete; // avoid assignment constructor
    map<int, vector<pair<string, int> > > netOpenMap_;
  };
```