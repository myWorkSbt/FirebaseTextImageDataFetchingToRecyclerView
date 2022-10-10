# FirebaseTextImageDataFetchingToRecyclerView     Using  FirebaseRecyclerAdapter

@Prerequistes 

  a>  connect firebase project to 
        a-i>>   realtime database
  
  b> add following dependency
       
    implementation 'com.google.firebase:firebase-database:20.0.6'
    implementation 'com.firebaseui:firebase-ui-database:8.0.2'
    implementation 'com.google.firebase:firebase-database:16.0.4'
    implementation 'de.hdodenhof:circleimageview:3.1.0'
    implementation 'com.github.bumptech.glide:glide:4.11.0'
    
    
   c>  In manifests.xml file  ,, enable  internet permissions . 
      
    <uses-permission android:name="android.permission.INTERNET" />

@# Step -1 > In  .java file
      
      a>> In onCreate()  function 
        mainBinding.recyclerView.setLayoutManager(new LinearLayoutManager(this));

        databaseReference = FirebaseDatabase.getInstance().getReference().child("student");

        FirebaseRecyclerOptions<FirebaseResponseModel> options =
                new FirebaseRecyclerOptions.Builder<FirebaseResponseModel>()
                        .setQuery(databaseReference, FirebaseResponseModel.class)
                        .build();
    
        myadapters = new RecyclerDataAdapter(options);
        mainBinding.recyclerView.setAdapter(myadapters);
   
   b>> Override onstart() and onStop() function outside the onCreate() funtion 
   
   
    @Override
    public void onStart() {
        super.onStart();
        myadapters.startListening();
    }

    @Override
    public void onStop() {
        super.onStop();
        myadapters.stopListening();
    }



@# Step-2 >  In RecyclerDataAdapter.java file 

public class RecyclerDataAdapter extends FirebaseRecyclerAdapter<FirebaseResponseModel,RecyclerDataAdapter.itemviewHolder> {

    public RecyclerDataAdapter( @NonNull FirebaseRecyclerOptions<FirebaseResponseModel>  mylist) {
        super(mylist);
    }
    @NonNull
    @Override
    public itemviewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view= LayoutInflater.from(parent.getContext()).inflate(R.layout.list_item_layout, parent,false); //  list_item_layout.xml is single row item layout 
        return new itemviewHolder(view);
    }



    @Override
    protected void onBindViewHolder(@NonNull itemviewHolder holder, int position, @NonNull FirebaseResponseModel model) {
        holder.password.setText(model.getPassword());
        holder.contactno.setText(model.getContact_no());
        holder.name.setText(model.getName());
        holder.age.setText(model.getAge());
        Glide.with(holder.user_img.getContext()).load(model.getImgLinks()).into(holder.user_img);
    }

    class itemviewHolder extends RecyclerView.ViewHolder{
        private CircleImageView user_img;
        private TextView name,contactno,password,age;
        public itemviewHolder(@NonNull View itemView) {
            super(itemView);
            user_img =(CircleImageView) itemView.findViewById(R.id.user_img);
            name= (TextView) itemView.findViewById(R.id.tv_name);
            password= (TextView) itemView.findViewById(R.id.tv_password);
            contactno= (TextView) itemView.findViewById(R.id.tv_contact_no);
            age= (TextView) itemView.findViewById(R.id.tv_age);
        }
    }
}        
