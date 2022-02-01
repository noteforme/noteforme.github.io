---
title: Dialog
comments: true
date: 2017-12-11 10:15:44
tags:
categories: ANDROID
---



#### Dialog
https://developer.android.com/guide/topics/ui/dialogs.html
https://developer.android.com/reference/android/app/DialogFragment.html

##### 　中间弹出

```
public class UpdateDialogFragment extends DialogFragment implements View.OnClickListener {
    public static final String UPDATE_MSG = "UPDATE_MSG";
    private UpdateBean updateMsg;


    public static  DialogFragment newInstance(UpdateBean updateBean) {
        UpdateDialogFragment upDialog = new UpdateDialogFragment();
        Bundle bundle = new Bundle();
        bundle.putParcelable(UPDATE_MSG, updateBean);
        upDialog.setArguments(bundle);
        return upDialog;
    }

    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {

         updateMsg = getArguments().getParcelable(UPDATE_MSG);

        AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
        // Get the layout inflater
        LayoutInflater inflater = getActivity().getLayoutInflater();
        // Inflate and set the layout for the dialog
        // Pass null as the parent view because its going in the dialog layout
        View view = inflater.inflate(R.layout.dialog_update, null);
        TextView tvUpdatetitle = (TextView) view.findViewById(R.id.tv_update_title);
        TextView tvUpDescribe = (TextView) view.findViewById(R.id.tv_up_describe);
        tvUpDescribe.setText(updateMsg.getDesc());
        TextView tvUpdateYes = (TextView) view.findViewById(R.id.tv_update_yes);
        TextView tvUpdateNo = (TextView) view.findViewById(R.id.tv_update_no);
        tvUpdateYes.setOnClickListener(this);
        tvUpdateNo.setOnClickListener(this);
        builder.setView(view);
        // Add action buttons
        return builder.create();
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.tv_update_yes:
                mListener.onDialogPositiveClick(this,updateMsg);
                break;
            case R.id.tv_update_no:
                mListener.onDialogNegativeClick(this);
                break;
        }
    }

    /* The activity that creates an instance of this dialog fragment must
     * implement this interface in order to receive event callbacks.
     * Each method passes the DialogFragment in case the host needs to query it. */
    public interface NoticeDialogListener {
        void onDialogPositiveClick(DialogFragment dialog,UpdateBean updateBean);

        void onDialogNegativeClick(DialogFragment dialog);
    }

    // Use this instance of the interface to deliver action events
    NoticeDialogListener mListener;

    // Override the Fragment.onAttach() method to instantiate the NoticeDialogListener
    @Override
    public void onAttach(Activity activity) {
        super.onAttach(activity);
        // Verify that the host activity implements the callback interface
        try {
            // Instantiate the NoticeDialogListener so we can send events to the host
            mListener = (NoticeDialogListener) activity;
        } catch (ClassCastException e) {
            // The activity doesn't implement the interface, throw exception
            throw new ClassCastException(activity.toString()
                    + " must implement NoticeDialogListener");
        }
    }
}

```

##### 底部弹出

https://github.com/umano/AndroidSlidingUpPanel
https://material.io/guidelines/components/dialogs.html#dialogs-specs

http://www.jianshu.com/p/0a7383e0ad0f



#### guide use

https://blog.csdn.net/u014626094/article/details/105430981



https://mp.weixin.qq.com/s/VadRKHau_YBsV8miFiggEw